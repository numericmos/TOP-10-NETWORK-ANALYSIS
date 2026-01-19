# TOP-10-NETWORK-ANALYSIS
library(ggplot2)
library(scales)

# Summarize total bytes per source IP
top_talkers <- aggregate(
  bytes_transferred ~ source_ip,
  data = traffic_data,
  FUN = sum
)

# Sort in descending order
top_talkers <- top_talkers[order(
  top_talkers$bytes_transferred,
  decreasing = TRUE
), ]

# Select top 10 talkers
top_talkers <- head(top_talkers, 10)

# Plot: Bar chart of top talkers
ggplot(top_talkers,
       aes(x = reorder(source_ip, bytes_transferred),
           y = bytes_transferred,
           fill = bytes_transferred)) +

  geom_bar(stat = "identity", color = "black") +
  scale_fill_gradient(low = "lightblue", high = "blue") +

  labs(
    title = "Top 10 Network Talkers",
    x = "Source IP Address",
    y = "Bytes Transferred"
  ) +

  geom_text(aes(label = comma(bytes_transferred)),
            vjust = -0.3, size = 3.5) +

  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(hjust = 0.5, size = 16, face = "bold"),
    axis.title.x = element_text(size = 12, face = "bold"),
    axis.title.y = element_text(size = 12, face = "bold"),
    legend.position = "none"
  )
