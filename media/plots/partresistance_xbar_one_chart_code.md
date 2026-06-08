# Filter the X025 dataset for Machine 1, Temperature 338, and Pressure 200
data_filtered <- X025 %>!
  filter(Machine == 1, Temperature == 338, Pressure == 200)

# Create an xbar.one control chart for PartResistance
qcc_object_partresistance <- qcc(data_filtered$PartResistance, type = 'xbar.one', plot = FALSE)

# Generate the plot using ggplot2 for better customization and then convert to plotly
df_plot <- data.frame(x = 1:length(data_filtered$PartResistance), y = data_filtered$PartResistance)

# Extract control limits and center line from qcc object
cl <- qcc_object_partresistance$center
lcl <- qcc_object_partresistance$limits[1]
ucl <- qcc_object_partresistance$limits[2]

p_partresistance_control <- ggplot(df_plot, aes(x = x, y = y)) +
  geom_line(color = '#0072B2') + 
  geom_point(color = '#0072B2', size = 2) +
  geom_hline(yintercept = cl, linetype = 'solid', color = '#000000', size = 1) + 
  geom_hline(yintercept = lcl, linetype = 'dashed', color = '#D55E00', size = 1) + 
  geom_hline(yintercept = ucl, linetype = 'dashed', color = '#D55E00', size = 1) + 
  annotate('text', x = max(df_plot$x), y = cl, label = 'CL', hjust = -0.5, vjust = -0.5, color = '#000000', size = 5) +
  annotate('text', x = max(df_plot$x), y = lcl, label = 'LCL', hjust = -0.5, vjust = -0.5, color = '#D55E00', size = 5) +
  annotate('text', x = max(df_plot$x), y = ucl, label = 'UCL', hjust = -0.5, vjust = 1.5, color = '#D55E00', size = 5) +
  labs(title = 'Xbar.one Chart for PartResistance', x = 'Sample Index', y = 'PartResistance') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_partresistance_control <- ggplotly(p_partresistance_control)
htmlwidgets::saveWidget(pg_partresistance_control, file = 'media/plots/partresistance_xbar_one_chart.html', selfcontained = TRUE)
