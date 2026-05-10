p_age <- ggplot(bigclass, aes(x = age)) +
  geom_histogram(binwidth = 1, fill = '#D55E00', color = 'white', alpha = 0.8) +
  labs(title = 'Distribution of Age', x = 'Age', y = 'Frequency') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg_age <- ggplotly(p_age)
htmlwidgets::saveWidget(pg_age, file = 'media/plots/age_histogram.html', selfcontained = TRUE)
