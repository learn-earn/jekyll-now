---
published: true
layout: post
title: How to apply shadow to a round cornered UIView with subviews contrainted to edges
excerpt: >-
  I was googling how to apply shadow to a round cornered UIView, and inspired from this...
---
I was googling how to apply shadow to a round cornered UIView, and  
inspired from this [post](https://medium.com/bytes-of-bits/swift-tips-adding-rounded-corners-and-shadows-to-a-uiview-691f67b83e4a).

And below is my version RoundShadowView so that you can create different RoundShadowViews with size of corner radius, shadow colour and background colour.

Furthermore, since the requirement is different from the original post, I have also improved the solution from the original post, so now when you add a subview constrained to the edges, it will still works well. The original solution won't fit the subview bounds into the rounded corner.

Instead of adding subviews to `RoundShadowView`, we add subviews to `RoundShadowView.containerView`. 

If there are better solutions, please get in touch, thank you for reading!

```
class RoundShadowView: UIView {
    private var shadowLayer: CAShapeLayer!
    private var cornerRadius: CGFloat = 25.0
    private var fillColor: UIColor = .blue // the color applied to the shadowLayer, rather than the view's backgroundColor
    private var shadowColor: UIColor = .black

    let containerView = UIView()
    
    init(frame: CGRect, cornerRadius: CGFloat, shadowColor: UIColor, backgroundColor: UIColor = UIColor.white) {
        super.init(frame: .zero)
        self.cornerRadius = cornerRadius
        self.fillColor = backgroundColor
        self.shadowColor = shadowColor
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func layoutSubviews() {
        super.layoutSubviews()
        
        containerView.translatesAutoresizingMaskIntoConstraints = false
        // set the cornerRadius of the containerView's layer
        containerView.layer.cornerRadius = cornerRadius
        containerView.layer.masksToBounds = true
        
        addSubview(containerView)
        
        NSLayoutConstraint.activate([
            containerView.leadingAnchor.constraint(equalTo: leadingAnchor),
            containerView.trailingAnchor.constraint(equalTo: trailingAnchor),
            containerView.topAnchor.constraint(equalTo: topAnchor),
            containerView.bottomAnchor.constraint(equalTo: bottomAnchor)
        ])
        
        if shadowLayer != nil {
            if shadowLayer.frame != frame {
                shadowLayer.removeFromSuperlayer()
                shadowLayer = nil
            }
        }
        if shadowLayer == nil {
            shadowLayer = CAShapeLayer()
            
            shadowLayer.path = UIBezierPath(roundedRect: bounds, cornerRadius: cornerRadius).cgPath
            shadowLayer.fillColor = fillColor.cgColor
            
            shadowLayer.shadowColor = shadowColor.cgColor
            shadowLayer.shadowPath = shadowLayer.path
            shadowLayer.shadowOffset = CGSize(width: 0.0, height: 1.0)
            shadowLayer.shadowOpacity = 0.3
            shadowLayer.shadowRadius = 3
            
            layer.insertSublayer(shadowLayer, at: 0)
        }
    }
}
```
