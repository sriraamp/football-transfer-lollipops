library(ggplot2)
library(dplyr)
library(forcats)
library(ggrepel)
library(tidyr)

# ---------------------------
# 1. Base Transfer Data
# ---------------------------
transfers <- data.frame(
  Year = 2000:2025,
  Player = c(
    "Luís Figo", "Zinedine Zidane", "Rio Ferdinand", "David Beckham", "Didier Drogba",
    "Michael Essien", "Andriy Shevchenko", "Fernando Torres", "Robinho", "Cristiano Ronaldo",
    "David Villa", "Fernando Torres", "Hulk", "Gareth Bale", "Ángel Di María",
    "Kevin De Bruyne", "Paul Pogba", "Neymar", "Kylian Mbappé", "João Félix",
    "Kai Havertz", "Jack Grealish", "Antony", "Enzo Fernández", "Jude Bellingham", "Khvicha Kvaratskhelia"
  ),
  AdjustedFee = c(
    539.5, 576.6, 328.0, 252.5, 252.0, 234.0, 260.0, 210.0, 225.0, 450.0,
    160.0, 220.0, 195.0, 280.0, 190.0, 177.0, 220.0, 420.0, 320.0, 210.0,
    123.0, 165.0, 123.0, 144.0, 112.0, 70.0
  ),
  FromClub = c(
    "Barcelona", "Juventus", "Leeds United", "Manchester United", "Marseille", "Lyon", "AC Milan",
    "Atlético Madrid", "Real Madrid", "Manchester United", "Valencia", "Liverpool", "Porto",
    "Tottenham Hotspur", "Real Madrid", "VfL Wolfsburg", "Juventus", "Barcelona", "Monaco", "Benfica",
    "Bayer Leverkusen", "Aston Villa", "Ajax", "Benfica", "Borussia Dortmund", "Napoli"
  ),
  ToClub = c(
    "Real Madrid", "Real Madrid", "Manchester United", "Real Madrid", "Chelsea", "Chelsea", "Chelsea",
    "Liverpool", "Manchester City", "Real Madrid", "Barcelona", "Chelsea", "Zenit",
    "Real Madrid", "Manchester United", "Manchester City", "Manchester United", "Paris Saint-Germain",
    "Paris Saint-Germain", "Atlético Madrid", "Chelsea", "Manchester City", "Manchester United", "Chelsea",
    "Real Madrid", "Paris Saint-Germain"
  )
)

# ---------------------------
# 2. Club Colors
# ---------------------------
club_colors <- c(
  "Real Madrid" = "#00529F", "Barcelona" = "#A50044", "Juventus" = "#000000",
  "Manchester United" = "#DA291C", "Chelsea" = "#034694", "AC Milan" = "#FB090B",
  "Atlético Madrid" = "#C8102E", "Manchester City" = "#6CABDD", "Liverpool" = "#C8102E",
  "Tottenham Hotspur" = "#132257", "Lyon" = "#002B6C", "Leeds United" = "#FFCD00",
  "Marseille" = "#00A0DF", "Valencia" = "#F4891C", "Porto" = "#2D3A84", "Zenit" = "#4298D1",
  "VfL Wolfsburg" = "#65B32E", "Monaco" = "#DA291C", "Benfica" = "#D71616",
  "Bayer Leverkusen" = "#E32219", "Ajax" = "#D3151B", "Borussia Dortmund" = "#FDE100",
  "Napoli" = "#007CC2", "Paris Saint-Germain" = "#004170", "Aston Villa" = "#670E36"
)

# ---------------------------
# 3. Expand into Long Format
# ---------------------------
library(tidyr)

transfers_long <- transfers %>%
  mutate(Player = fct_reorder(Player, AdjustedFee),
         Midpoint = AdjustedFee / 2) %>%
  select(Player, AdjustedFee, Midpoint, FromClub, ToClub) %>%
  pivot_longer(cols = c(FromClub, ToClub),
               names_to = "Role", values_to = "Club") %>%
  mutate(
    y_start = ifelse(Role == "FromClub", 0, Midpoint),
    y_end   = ifelse(Role == "FromClub", Midpoint, AdjustedFee)
  )

# ---------------------------
# 4. Final Plot
# ---------------------------
ggplot(transfers_long) +
  geom_segment(aes(x = Player, xend = Player, y = y_start, yend = y_end, color = Club), size = 1.5) +
  geom_point(data = transfers, aes(x = Player, y = AdjustedFee), color = "black", size = 3) +
  geom_text_repel(data = transfers, aes(x = Player, y = AdjustedFee, label = paste0("€", AdjustedFee, "M")),
                  size = 3.5, fontface = "bold", nudge_y = 20, direction = "y", max.overlaps = 30) +
  scale_color_manual(values = club_colors, name = "Club") +
  labs(
    title = "World Record Football Transfers (2000–2025)",
    subtitle = "Each lollipop shows the selling club (bottom) and buying club (top) by color",
    x = "Player",
    y = "Adjusted Fee (€ million)"
  ) +
  theme_minimal(base_size = 13) +
  theme(
    plot.title = element_text(face = "bold", size = 15),
    axis.text.x = element_text(angle = 45, hjust = 1),
    legend.position = "right",
    panel.grid.major.x = element_blank()
  )
