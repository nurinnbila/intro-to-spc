# Filter the X025 dataset for Machine 2, Temperature 338, and Pressure 200
data_filtered_m2 <- X025 %>% filter(Machine == 2, Temperature == 338, Pressure == 200)

# Create an xbar.one control chart for PartResistance
qcc_object_partresistance_m2 <- qcc(data_filtered_m2$PartResistance, type = 'xbar.one', plot = FALSE)

# Generate the plot using ggplot2 for better customization and then convert to plotly
df_plot_m2 <- data.frame(x = 1:length(data_filtered_m2$PartResistance), y = data_filtered_m2$PartResistance)

# Extract control limits and center line from qcc object
cl_m2 <- qcc_object_partresistance_m2$center
lcl_m2 <- qcc_object_partresistance_m2$limits[1]
ucl_m2 <- qcc_object_partresistance_m2$limits[2]

p_partresistance_control_m2 <- ggplot(df_plot_m2, aes(x = x, y = y)) +
  geom_line(color = '#0072B2') + 
  geom_point(color = '#0072B2', size = 2) +
  geom_hline(yintercept = cl_m2, linetype = 'solid', color = '#000000', size = 1) + 
  geom_hline(yintercept = lcl_m2, linetype = 'dashed', color = '#D55E00', size = 1) + 
  geom_hline(yintercept = ucl_m2, linetype = 'dashed', color = '#D55E00', size = 1) + 
  annotate('text', x = max(df_plot_m2$x), y = cl_m2, label = 'CL', hjust = -0.5, vjust = -0.5, color = '#000000', size = 5) +
  annotate('text', x = max(df_plot_m2$x), y = lcl_m2, label = 'LCL', hjust = -0.5, vjust = -0.5, color = '#D55E00', size = 5) +
  annotate('text', x = max(df_plot_m2$x), y = ucl_m2, label = 'UCL', hjust = -0.5, vjust = 1.5, color = '#D55E00', size = 5) +
  labs(title = 'Control Chart for Part Resistance (Machine 2, P=200kPa, T=338K)', x = 'Sample Index', y = 'PartResistance') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_partresistance_control_m2 <- ggplotly(p_partresistance_control_m2)
htmlwidgets::saveWidget(pg_partresistance_control_m2, file = 'media/plots/partresistance_xbar_one_chart_m2.html', selfcontained = TRUE)
