# CALayer 🆚 CGLayer 차이점

- CALayer: 뷰에서 이미지 기반 컨텐츠 처리에 사용, 이미지에 애니메이션을 줄 수 있는 수단 제공
- CGLayer: 컨텐츠를 여러번 그려야 할 때, 렌더링을 최적화하기 위해서 사용

### 언제 사용?

1. **동일한 컨텐츠를 여러번 그릴 때**
- 동일한 컨텐츠를 CALaye에 담아두면, **`CGContextDrawLayerAtPoint()`** or **`CGContextDrawLayerInRect()`**를 이용해서 효율적으로 그릴 수 있다.

```swift
private func drawSameContentsRepeatedly() {
    let size: CGSize = .init(width: 100, height: 100)
    if let layerContext = CGContext(data: nil, width: Int(size.width), height: Int(size.height), bitsPerComponent: 8, bytesPerRow: 0, space: CGColorSpaceCreateDeviceRGB(), bitmapInfo: CGImageAlphaInfo.premultipliedLast.rawValue) {
        
        let layer = CGLayer(layerContext, size: size, auxiliaryInfo: nil)
        
        let layerContext = layer?.context
        
        // Draw something
        layerContext?.setFillColor(UIColor.red.cgColor)
        layerContext?.fill(CGRect(origin: .zero, size: size))
        
        // Use the layer
        UIGraphicsBeginImageContext(size) // bitmap 기반 이미지를 만들기 위한 context 시작
        let context = UIGraphicsGetCurrentContext()
        
        for _ in 1...10 {
            // 실제로 그리는 작업 - 현재 context에 CGLayer를 10번 그린다.
            context?.draw(layer!, in: CGRect(origin: .zero, size: size))
        }
        
        let img = UIGraphicsGetImageFromCurrentImageContext() // 현재 context에 그려진 이미지 추출 
        UIGraphicsEndImageContext() // bitmap 기반 이미지를 만들기 위한 context 끝
        
        let imageView: UIImageView = .init(image: img)
        imageView.frame = .init(origin: .init(x: 100, y: 100), size: size)
        view.addSubview(imageView)
    }
}
```

1. **Offscreen Drawing**
- **화면에 표시되기 전에**, 화면 밖에서 보여지지 않을 때, 텍스트나 그림을 구성할 때 사용
- ex) 복잡한 그림의 경우 화면 밖에서 구성 후에, 화면에 표시해야 하는 경우 존재
- ex) 그리기 애플리케이션의 undo 기능에서, CGLayer를 사용해서 각각의 작업마다 그리기 상태를 저장할 수 있다.

```swift
private func drawAtOffScreen() {
    let size: CGSize = .init(width: 100, height: 100)
    if let layerContext = CGContext(data: nil, width: 100, height: 100, bitsPerComponent: 8, bytesPerRow: 0, space: CGColorSpaceCreateDeviceRGB()
                                    , bitmapInfo: CGImageAlphaInfo.premultipliedLast.rawValue) {
        let layer = CGLayer(layerContext, size: size, auxiliaryInfo: nil)
        let layerContext = layer?.context
        
        // Draw Something
        layerContext?.setFillColor(UIColor.orange.cgColor)
        layerContext?.fill(CGRect(origin: .zero, size: size))
        
        // not use layer, save the layer for layer use
        
        // Later on, draw the layer into the visible context - 그릴 때 사용
       let visibleContext = UIGraphicsGetCurrentContext()
        // 실제로 그리는 작업
       visibleContext?.draw(layer!, in: CGRect(origin: .zero, size: size))
    }
}
```

1. **Cached Drawing**
- 렌더링하는데 시간이 오래 걸리고, 자주 변경되지 않는 컨텐츠 복잡한 UI를 CGLayer에 한번 그린다음, 나중에 그리는 것이 효율적일 수 있다.

1. **Complex Clipping Paths(복잡한 클리핑 경로)**
- 여러 곳에서 재사용되는 복잡한 클리핑 경로가 있는 경우 CGLayer를 생성하고 클리핑 경로를 한 번 설정한 다음 해당 레이어를 렌더링에 사용하는 것이 더 효율적일 수 있습니다.

### ⚠️ 주의 사항

- CGLayer는 메모리를 소비
    - 높이, 너비, 픽셀당 비트수에 비례해서 메모리 사용
- 모든 상황에 적합X, 경우에 따라서는 매번 컨텐츠를 다시 그리는 것이 효율적이다.