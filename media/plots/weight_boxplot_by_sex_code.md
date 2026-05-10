p_weight_sex <- ggplot(bigclass, aes(x = sex, y = weight, fill = sex)) +
  geom_boxplot(alpha = 0.8) +
  scale_fill_manual(values = c('F' = '#009E73', 'M' = '#0072B2')) +
  labs(title = 'Weight Distribution by Sex', x = 'Sex', y = 'Weight') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white'),
    legend.position = 'none'
  )

pg_weight_sex <- ggplotly(p_weight_sex)
htmlwidgets::saveWidget(pg_weight_sex, file = 'media/plots/weight_boxplot_by_sex.html', selfcontained = TRUE)
