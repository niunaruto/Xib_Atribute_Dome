# Xib_Atribute_Dome
一个 XIB iOS 适配思路 demo

# ViewController.Swift 文件

import UIKit
import SnapKit
class ViewController: UIViewController {

    @IBOutlet weak var label: UILabel!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        DispatchQueue.main.asyncAfter(deadline:.now() + 3) {
            self.label.snp.updateConstraints({ (make) in
                make.left.equalToSuperview().offset(20*pointRatio)
                make.top.equalToSuperview().offset(200*pointRatio)
            })
        }
    }
}

# UIView-Extension.Swift 文件



import UIKit
var ratioKey = 101
let pointRatio : CGFloat = UIScreen.main.bounds.size.width / 750 * 2

extension UIView {

    @IBInspectable var ratio: Bool {
        set {
            objc_setAssociatedObject(self, &ratioKey, newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_ASSIGN)
        }
        get {
            if let ratio = objc_getAssociatedObject(self, &ratioKey) as? Bool {
                return ratio
            }
            return false
        }
    }
    
    
 
    
    open override func awakeFromNib() {
        super.awakeFromNib()
        
        if ratio {
            for layout in self.constraints {
                layout.constant = layout.constant * pointRatio
            }
            
            if self.isKind(of: UILabel.classForCoder())  {
                guard let label = self as? UILabel else {
                    return
                }
                label.font = UIFont.init(name: label.font.fontName, size: label.font.pointSize * pointRatio)
            }else if self.isKind(of: UIButton.classForCoder()) {
                guard let button = self as? UIButton else {
                    return
                }
                guard let titleLabel = button.titleLabel else {
                    return
                }
                titleLabel.font = UIFont.init(name: titleLabel.font.fontName, size: titleLabel.font.pointSize * pointRatio)
                
            }
        }
      
        
    }
}


# UILabel-Extension.Swift

import UIKit
var fontnameKey = "101"

extension UILabel {
    
    @IBInspectable var fontname: String {
        set {
            objc_setAssociatedObject(self, &fontnameKey, newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_COPY_NONATOMIC)
            self.font = UIFont.init(name: fontname, size: self.font.pointSize)

        }
        get {
            if let fontname = objc_getAssociatedObject(self, &fontnameKey) as? String {
                return fontname
            }
            return ""
        }
    }
    
}



  
    



