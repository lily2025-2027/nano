% Parameters
Vth = 1; % Threshold voltage in volts
k = 0.5e-3; % Transconductance parameter (A/V^2)

VGS_values = [2 3 4]; % Gate-Source voltages to simulate
VDS = linspace(0, 5, 100); % Drain-Source voltage sweep

% Initialize plot
figure;
hold on;
colors = ['r', 'g', 'b'];

% Loop through each VGS and calculate ID
for idx = 1:length(VGS_values)
    VGS = VGS_values(idx);
    ID = zeros(size(VDS));

    for i = 1:length(VDS)
        if VGS <= Vth
            ID(i) = 0; % Cutoff
        elseif VDS(i) < VGS - Vth
            % Triode Region
            ID(i) = k * ((VGS - Vth) * VDS(i) - 0.5 * VDS(i)^2);
        else
            % Saturation Region
            ID(i) = 0.5 * k * (VGS - Vth)^2;
        end
    end

    plot(VDS, ID, colors(idx), 'LineWidth', 2, 'DisplayName', ['V_{GS} = ' num2str(VGS) 'V']);
end
