p <- ggplot(bigclass, aes(x = Math)) +
  geom_histogram(binwidth = 50, fill = '#0072B2', color = 'white', alpha = 0.8) +
  labs(title = 'Distribution of Math Scores', x = 'Math Score', y = 'Frequency') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white')
  )

pg <- ggplotly(p)
htmlwidgets::saveWidget(pg, file = 'media/plots/math_scores_histogram.html', selfcontained = TRUE)
