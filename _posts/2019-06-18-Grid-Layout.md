---
title:  "Grid Layout"
tags: Grid layout
---

### Day 14:

When learning Angular, I found that Grid layout and Bootstrap are very fundamental knowledge nowadays. In a 
responsive and self-adjusted website, these are necessary skills.

##  Tech trend:

table -> float/position/inline -> FlexBox -> Grid

We can combine FlexBox and Grid technology together.

## Terminologies

* Grid Containers
* Grid items
* Grid lines
* Grid tracks
* Grid cells
* Grid areas
* fr(Unit): fraction

## Attributes

### Display 

```css
.container {
    display: grid;
}
```

### grid-template

grid-template-columns: defines the number of columns in your grid layout, and it can define the width of each column.

Examples:

```css
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto auto;
}
```

```css
.grid-container {
  display: grid;
  grid-template-columns: 80px 200px auto 40px;
}
```

```css
.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
    grid-auto-rows: auto auto auto;
}
```

### Naming grid lines

```css
.container {
    grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px;
}
```

### grid-template-areas

```css
.container {
grid-template-areas: "header header header header"
                     "main main . sidebar"
                     "footer footer footer footer";
}
```

### grid-column-gap / grid-row-gap

```css
.container {
grid-column-gap: <line-size>;
grid-gap: <row-gap> <column-gap>;
}
```

## Items

Start, end, center, stretch

* justify-items: horizontal alignment
* align-items: vertical alignment
* place-items: first vertical second horizontal

```css
.container{
justify-items: start;
place-items: center;
}
```






