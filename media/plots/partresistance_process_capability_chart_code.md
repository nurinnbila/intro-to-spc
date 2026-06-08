# Filter the X025 dataset for Machine 1, Temperature 338, and Pressure 200
data_filtered <- X025 %>% filter(Machine == 1, Temperature == 338, Pressure == 200)

# Define assumed specification limits for PartResistance (LSL, USL, Target)
lsl <- 5
usl <- 7
target <- 6

# Calculate mean and standard deviation of PartResistance from the filtered data
mean_resistance <- mean(data_filtered$PartResistance)
sd_resistance <- sd(data_filtered$PartResistance)

p_capability <- ggplot(data_filtered, aes(x = PartResistance)) +
  geom_histogram(aes(y = after_stat(density)), binwidth = 0.1, fill = '#0072B2', color = 'white', alpha = 0.8) +
  stat_function(fun = dnorm, args = list(mean = mean_resistance, sd = sd_resistance), color = '#D55E00', linewidth = 2) +
  geom_vline(xintercept = lsl, linetype = 'dashed', color = '#CC79A7', size = 1) +
  geom_vline(xintercept = usl, linetype = 'dashed', color = '#CC79A7', size = 1) +
  geom_vline(xintercept = target, linetype = 'solid', color = '#000000', size = 1) +
  annotate('text', x = lsl, y = max(density(data_filtered$PartResistance)$y) * 1.05, label = 'LSL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = usl, y = max(density(data_filtered$PartResistance)$y) * 1.05, label = 'USL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = target, y = max(density(data_filtered$PartResistance)$y) * 1.15, label = 'Target', hjust = 0.5, vjust = 0, color = '#000000', size = 5) +
  labs(title = 'Process Capability Chart for Part Resistance (Machine 1, P=200kPa, T=338K)', x = 'PartResistance', y = 'Density') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_capability <- ggplotly(p_capability)
htmlwidgets::saveWidget(pg_capability, file = 'media/plots/partresistance_process_capability_chart.html', selfcontained = TRUE)
