# Filter the X025 dataset for Machine 2, Temperature 338, and Pressure 200
data_filtered_m2_partlength <- X025 %>% filter(Machine == 2, Temperature == 338, Pressure == 200)

# Create an xbar.one control chart for PartLength
qcc_object_partlength_m2 <- qcc(data_filtered_m2_partlength$PartLength, type = 'xbar.one', plot = FALSE)

# Generate the plot using ggplot2 for better customization and then convert to plotly
df_plot_m2_partlength <- data.frame(x = 1:length(data_filtered_m2_partlength$PartLength), y = data_filtered_m2_partlength$PartLength)

# Extract control limits and center line from qcc object
cl_m2_partlength <- qcc_object_partlength_m2$center
lcl_m2_partlength <- qcc_object_partlength_m2$limits[1]
ucl_m2_partlength <- qcc_object_partlength_m2$limits[2]

p_partlength_control_m2 <- ggplot(df_plot_m2_partlength, aes(x = x, y = y)) +
  geom_line(color = '#0072B2') + 
  geom_point(color = '#0072B2', size = 2) +
  geom_hline(yintercept = cl_m2_partlength, linetype = 'solid', color = '#000000', size = 1) + 
  geom_hline(yintercept = lcl_m2_partlength, linetype = 'dashed', color = '#D55E00', size = 1) + 
  geom_hline(yintercept = ucl_m2_partlength, linetype = 'dashed', color = '#D55E00', size = 1) + 
  annotate('text', x = max(df_plot_m2_partlength$x), y = cl_m2_partlength, label = 'CL', hjust = -0.5, vjust = -0.5, color = '#000000', size = 5) +
  annotate('text', x = max(df_plot_m2_partlength$x), y = lcl_m2_partlength, label = 'LCL', hjust = -0.5, vjust = -0.5, color = '#D55E00', size = 5) +
  annotate('text', x = max(df_plot_m2_partlength$x), y = ucl_m2_partlength, label = 'UCL', hjust = -0.5, vjust = 1.5, color = '#D55E00', size = 5) +
  labs(title = 'Control Chart for Part Length (Machine 2, P=200kPa, T=338K)', x = 'Sample Index', y = 'PartLength') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_partlength_control_m2 <- ggplotly(p_partlength_control_m2)
htmlwidgets::saveWidget(pg_partlength_control_m2, file = 'media/plots/partlength_xbar_one_chart_m2.html', selfcontained = TRUE)
