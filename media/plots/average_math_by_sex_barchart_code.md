avg_math_by_sex <- bigclass %>!
  group_by(sex) %>!
  summarise(average_math = mean(Math)) %>!
  ungroup()

p_math_sex <- ggplot(avg_math_by_sex, aes(x = sex, y = average_math, fill = sex)) +
  geom_bar(stat = 'identity', color = 'white', alpha = 0.8) +
  scale_fill_manual(values = c('F' = '#009E73', 'M' = '#0072B2')) +
  labs(title = 'Average Math Score by Sex', x = 'Sex', y = 'Average Math Score') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white'),
    legend.position = 'none'
  )
