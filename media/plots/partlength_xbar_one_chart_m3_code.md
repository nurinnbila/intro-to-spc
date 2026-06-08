# Filter the X025 dataset for Machine 3, Temperature 338, and Pressure 200
data_filtered_m3_pl <- X025 %>% filter(Machine == 3, Temperature == 338, Pressure == 200)

# Create an xbar.one control chart for PartLength
qcc_object_partlength_m3 <- qcc(data_filtered_m3_pl$PartLength, type = 'xbar.one', plot = FALSE)

# Generate the plot using ggplot2 for better customization and then convert to plotly
df_plot_m3_pl <- data.frame(x = 1:length(data_filtered_m3_pl$PartLength), y = data_filtered_m3_pl$PartLength)

# Extract control limits and center line from qcc object
cl_m3_pl <- qcc_object_partlength_m3$center
lcl_m3_pl <- qcc_object_partlength_m3$limits[1]
ucl_m3_pl <- qcc_object_partlength_m3$limits[2]

p_partlength_control_m3 <- ggplot(df_plot_m3_pl, aes(x = x, y = y)) +
  geom_line(color = '#0072B2') + 
  geom_point(color = '#0072B2', size = 2) +
  geom_hline(yintercept = cl_m3_pl, linetype = 'solid', color = '#000000', size = 1) + 
  geom_hline(yintercept = lcl_m3_pl, linetype = 'dashed', color = '#D55E00', size = 1) + 
  geom_hline(yintercept = ucl_m3_pl, linetype = 'dashed', color = '#D55E00', size = 1) + 
  annotate('text', x = max(df_plot_m3_pl$x), y = cl_m3_pl, label = 'CL', hjust = -0.5, vjust = -0.5, color = '#000000', size = 5) +
  annotate('text', x = max(df_plot_m3_pl$x), y = lcl_m3_pl, label = 'LCL', hjust = -0.5, vjust = -0.5, color = '#D55E00', size = 5) +
  annotate('text', x = max(df_plot_m3_pl$x), y = ucl_m3_pl, label = 'UCL', hjust = -0.5, vjust = 1.5, color = '#D55E00', size = 5) +
  labs(title = 'Control Chart for Part Length (Machine 3, P=200kPa, T=338K)', x = 'Sample Index', y = 'PartLength') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_partlength_control_m3 <- ggplotly(p_partlength_control_m3)
htmlwidgets::saveWidget(pg_partlength_control_m3, file = 'media/plots/partlength_xbar_one_chart_m3.html', selfcontained = TRUE)
