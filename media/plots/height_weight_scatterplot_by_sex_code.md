p_height_weight_sex <- ggplot(bigclass, aes(x = height, y = weight, color = sex)) +
  geom_point(alpha = 0.8, size = 3) +
  scale_color_manual(values = c('F' = '#009E73', 'M' = '#0072B2')) +
  labs(title = 'Height vs. Weight by Sex', x = 'Height (inches)\, y = 'Weight (pounds)') +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 18, face = 'bold'),
    axis.title = element_text(size = 18),
    axis.text = element_text(size = 14),
    panel.background = element_rect(fill = 'white', colour = 'white'),
    plot.background = element_rect(fill = 'white', colour = 'white'),
    legend.position = 'bottom'
  )

pg_height_weight_sex <- ggplotly(p_height_weight_sex)
htmlwidgets::saveWidget(pg_height_weight_sex, file = 'media/plots/height_weight_scatterplot_by_sex.html', selfcontained = TRUE)
