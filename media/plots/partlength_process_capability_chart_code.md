# Filter the X025 dataset for Machine 1, Temperature 338, and Pressure 200
data_filtered <- X025 %>% filter(Machine == 1, Temperature == 338, Pressure == 200)

# Define assumed specification limits for PartLength (LSL, USL, Target)
lsl <- 45
usl <- 55
target <- 50

# Calculate mean and standard deviation of PartLength from the filtered data
mean_partlength <- mean(data_filtered$PartLength)
sd_partlength <- sd(data_filtered$PartLength)

# Generate the histogram with overlaid normal distribution and specification limits
p_capability_partlength <- ggplot(data_filtered, aes(x = PartLength)) +
  geom_histogram(aes(y = after_stat(density)), binwidth = 0.5, fill = '#0072B2', color = 'white', alpha = 0.8) +
  stat_function(fun = dnorm, args = list(mean = mean_partlength, sd = sd_partlength), color = '#D55E00', linewidth = 1.2) +
  geom_vline(xintercept = lsl, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = usl, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = target, linetype = 'solid', color = '#000000', linewidth = 1) +
  annotate('text', x = lsl, y = max(density(data_filtered$PartLength)$y) * 1.05, label = 'LSL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = usl, y = max(density(data_filtered$PartLength)$y) * 1.05, label = 'USL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = target, y = max(density(data_filtered$PartLength)$y) * 1.15, label = 'Target', hjust = 0.5, vjust = 0, color = '#000000', size = 5) +
  labs(title = 'Process Capability Chart for PartLength', x = 'PartLength', y = 'Density') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_capability_partlength <- ggplotly(p_capability_partlength)
htmlwidgets::saveWidget(pg_capability_partlength, file = 'media/plots/partlength_process_capability_chart.html', selfcontained = TRUE)
