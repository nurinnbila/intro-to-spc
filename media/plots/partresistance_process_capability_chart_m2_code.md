# Filter the X025 dataset for Machine 2, Temperature 338, and Pressure 200
data_filtered_m2_pr_pc <- X025 %>% filter(Machine == 2, Temperature == 338, Pressure == 200)

# Define assumed specification limits for PartResistance (LSL, USL, Target)
lsl_pr_m2 <- 5
usl_pr_m2 <- 7
target_pr_m2 <- 6

# Calculate mean and standard deviation of PartResistance from the filtered data
mean_partresistance_m2 <- mean(data_filtered_m2_pr_pc$PartResistance)
sd_partresistance_m2 <- sd(data_filtered_m2_pr_pc$PartResistance)

# Generate the histogram with overlaid normal distribution and specification limits
p_capability_partresistance_m2 <- ggplot(data_filtered_m2_pr_pc, aes(x = PartResistance)) +
  geom_histogram(aes(y = after_stat(density)), binwidth = 0.1, fill = '#0072B2', color = 'white', alpha = 0.8) +
  stat_function(fun = dnorm, args = list(mean = mean_partresistance_m2, sd = sd_partresistance_m2), color = '#D55E00', linewidth = 1.2) +
  geom_vline(xintercept = lsl_pr_m2, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = usl_pr_m2, linetype = 'dashed', color = '#CC79A7', linewidth = 1) +
  geom_vline(xintercept = target_pr_m2, linetype = 'solid', color = '#000000', linewidth = 1) +
  annotate('text', x = lsl_pr_m2, y = max(density(data_filtered_m2_pr_pc$PartResistance)$y) * 1.05, label = 'LSL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = usl_pr_m2, y = max(density(data_filtered_m2_pr_pc$PartResistance)$y) * 1.05, label = 'USL', hjust = 0.5, vjust = 0, color = '#CC79A7', size = 5) +
  annotate('text', x = target_pr_m2, y = max(density(data_filtered_m2_pr_pc$PartResistance)$y) * 1.15, label = 'Target', hjust = 0.5, vjust = 0, color = '#000000', size = 5) +
  labs(title = 'Process Capability Chart for Part Resistance (Machine 2, P=200kPa, T=338K)', x = 'PartResistance', y = 'Density') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_capability_partresistance_m2 <- ggplotly(p_capability_partresistance_m2)
htmlwidgets::saveWidget(pg_capability_partresistance_m2, file = 'media/plots/partresistance_process_capability_chart_m2.html', selfcontained = TRUE)
