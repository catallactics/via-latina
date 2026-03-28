# Image Detection Logic

> **Core logic inlined in SKILL.md** Step 4 `Image Detection` section.

## Detection Keywords

Hardware: breadboard, circuit, connection, wiring, pin, LED, resistor, sensor, motor, diagram, soldering, capacitor, transistor, relay, GND, VCC
Visual: UI, screen, layout, wireframe, mockup, screenshot
Data: graph, chart, flowchart, architecture, ERD

Korean equivalents: 브레드보드, 회로, 연결, 배선, 핀, 센서, 모터, 점퍼선, 납땜, 화면, 레이아웃, 그래프

## Placeholder Format

```markdown
> [!image-needed] {description}
> - Keywords: {detected keywords}
> - ASCII: {yes/no}
> - Recommended: {photo type}
```

## Rules

1. End of section, one per section, skip if image exists
2. If ASCII exists → note "photo recommended"
3. Searchable: `grep -r "\[!image-needed\]" vault/`
