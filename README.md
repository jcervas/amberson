# amberson


# Amberson Election Status

```{r function-units}
# Function to print units by row based on the first digit
print_units_by_row <- function(units) {
  first_digits <- unique(floor(units / 100))
  for (digit in first_digits) {
    cat(units[floor(units / 100) == digit], "\n")
  }
}
```


```{r}
registration <- read.csv("/Users/cervas/My Drive/Amberson/Elections/2024/2024-05-17-atca-voter-list-v3.csv")
head(registration)
```

# Total Units Voted
```{r read-OpaVote}
voted <- read.table("/Users/cervas/Library/Mobile Documents/com~apple~CloudDocs/Downloads/voted.txt")
voted$voted <- 1

not_voted <- read.table('/Users/cervas/Library/Mobile Documents/com~apple~CloudDocs/Downloads/not_voted.txt')
not_voted$not_voted <- 1

voted_merge <- merge(registration,voted, by.x="Email", by.y="V1", all=T)
units_voted <- sort(voted_merge$Unit[voted_merge$voted %in% 1])

# Exclude those who are disabled (not usual)
disabled_voted <- read.table('/Users/cervas/Library/Mobile Documents/com~apple~CloudDocs/Downloads/disabled.txt')
disabled_voted$disabled <- 1

# Not voted
voted_notvoted_merge <- merge(voted_merge,not_voted, by.x="Email", by.y="V1", all=T)
voted_notvoted_disabled_merge <- merge(voted_notvoted_merge,disabled_voted, by.x="Email", by.y="V1", all.x=T)

units_notvoted <- sort(voted_notvoted_disabled_merge$Unit[is.na(voted_notvoted_disabled_merge$disable) & is.na(voted_notvoted_disabled_merge$voted)])

# Visited OpaVote?
# visited <- read.table('/Users/cervas/Library/Mobile Documents/com~apple~CloudDocs/Downloads/visited.txt')
# visited$visited <- 1
# voted_notvoted_disabled_visited_merge <- merge(voted_notvoted_disabled_merge,visited, by.x="Email", by.y="V1", all.x=T)
# sort(voted_notvoted_disabled_visited_merge$Unit[
#   (is.na(voted_notvoted_disabled_visited_merge$disable) & 
#    is.na(voted_notvoted_disabled_visited_merge$voted)) & 
#   !is.na(voted_notvoted_disabled_visited_merge$visited)
# ])

```



# Number of voters
```{r voted-output}
sum(voted$voted)
```

# Unit Percentage (Weighted) voted"
```{r}
paste0(round(sum(voted_merge$Weight[voted_merge$voted %in% 1])/999989, d=3) * 100, "%")
```

# Units that have voted
```{r}
print_units_by_row(units_voted)
```

# Units that have not voted
```{r}
print_units_by_row(units_notvoted)
```






