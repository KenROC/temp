```
<Chart
  dataSource={data}
  rotated={false}
  adaptiveLayout={{
    width: 600,
    height: 400,
  }}
>
  <ArgumentAxis>
    <Label
      displayMode="stagger"
      staggeringSpacing={12}
      overlappingBehavior="hide"
    />
  </ArgumentAxis>
</Chart>
```
---
| 設定                       | 效果                 |
| ------------------------ | ------------------ |
| `displayMode="rotate"`   | 旋轉標籤，例如 45 度       |
| `displayMode="stagger"`  | 將不同的 X 軸標籤交錯排成上下兩排 |
| `staggeringSpacing={10}` | 設定上下兩排之間的距離        |
| `displayMode="standard"` | 一般單排顯示             |