# Filter the X025 dataset for Machine 3, Temperature 338, and Pressure 200
data_filtered_m3_pl_pc <- X025 %>% filter(Machine == 3, Temperature == 338, Pressure == 200)

# Define assumed specification limits for PartLength (LSL, USL, Target)
lsl_pl_m3 <- 45
usl_pl_m3 <- 55
target_pl_m3 <- 50

# Calculate mean and standard deviation of PartLength from the filtered data
mean_partlength_m3 <- mean(data_filtered_m3_pl_pc$PartLength)
sd_partlength_m3 <- sd(data_filtered_m3_pl_pc$PartLength)

# Generate the histogram with overlaid normal distribution and specification limits
p_capability_partlength_m3 <- ggplot(data_filtered_m3_pl_pc, aes(x = PartLength)) +
  geom_histogram(aes(y = after_stat(density)), binwidth = 0.5, fill = '#0072B2', color = 'white', alpha = 0.8) +
  stat_function(fun = dnorm, args = list(mean = mean_partlength_m3, sd = sd_partlength_m3), color = '#D55E00', linewidth = 1.2) +
  geom_vline(xintercept = lsl_pl_m3, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = usl_pl_m3, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = target_pl_m3, linetype = 'solid', color = '#000000', linewidth = 1) +
  annotate('text', x = lsl_pl_m3, y = max(density(data_filtered_m3_pl_pc$PartLength)$y) * 1.05, label = 'LSL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = usl_pl_m3, y = max(density(data_filtered_m3_pl_pc$PartLength)$y) * 1.05, label = 'USL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = target_pl_m3, y = max(density(data_filtered_m3_pl_pc$PartLength)$y) * 1.15, label = 'Target', hjust = 0.5, vjust = 0, color = '#000000', size = 5) +
  labs(title = 'Process Capability Chart for PartLength (Machine 3, P=200kPa, T=338K)', x = 'PartLength', y = 'Density') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_capability_partlength_m3 <- ggplotly(p_capability_partlength_m3)
htmlwidgets::saveWidget(pg_capability_partlength_m3, file = 'media/plots/partlength_process_capability_chart_m3.html', selfcontained = TRUE)
