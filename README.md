# Gesture
Integrated click, double click, drag, long press, zoom and other gestures, encapsulates multiple views.
##
/***
 1、UITapGestureRecognizer （点击）
 
 2、UIPinchGestureRecognizer（双指）
 
 3、UIRotationGestureRecognizer（旋转）
 
 4、UISwipeGestureRecognizer（滑动，快速移动）
 
 5、UIPanGestureRecognizer（拖动，慢速移动）
 
 6、UILongPressGestureRecognizer（长按）
 ****/
 ##/
 
// 手势的生成和方法看具体代码

 ##
 /****
       封装了一个方法判断手势操作的view
 *****/
 
 -(UIView *)selectView:(UIView *)selectView{
    NSInteger  tag = selectView.tag;
    if (tag == 101) {
        selectView = self.gestureView;
    }else if (tag == 102){
        selectView = self.gestureView2;
    }else if (tag == 103){
        selectView = self.gestureView3;
    }
    return selectView;

}
 
 ##
 (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{

    // 找到点击的view
    UITouch *touch = [touches anyObject];
    CGPoint  point = [touch locationInView:self.view];
    UIView * fitView = [self.view hitTest:point withEvent:event];
    [self selectView:fitView];
    
    NSLog(@"pot--frame--%@",NSStringFromCGRect(fitView.frame));
    originalLocation = [touch locationInView:fitView];
    NSLog(@"pot--origin--%@",NSStringFromCGPoint(originalLocation));
}
##
-(void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
     
    UITouch *touch = [touches anyObject];
    CGPoint  point = [touch locationInView:self.view];
    UIView * fitView = [self.view hitTest:point withEvent:event];
    [self selectView:fitView];
    
    CGPoint currentLocation = [touch locationInView:fitView];
    CGPoint beformLocation = [touch previousLocationInView:fitView];
    
    float x = 0.0;
   
    float y = 0.0;
    
    if (fitView.tag == 101 || fitView.tag ==102 ||fitView.tag == 103 ) {
        x = currentLocation.x- beformLocation.x;
        y = currentLocation.y- beformLocation.y;
    }
    
    CGFloat  width  = fitView.frame.size.width;
    CGFloat  height = fitView.frame.size.height;
    CGFloat  originX = fitView.frame.origin.x;
    CGFloat  originY = fitView.frame.origin.y;
    
    NSLog(@"point--originalLocation--%@",NSStringFromCGPoint(originalLocation));
    NSLog(@"point--frame--%@",NSStringFromCGRect(fitView.frame));
    

    CGFloat  widthPan = 200 / 4.0;
    CGFloat  heightPan = 200 / 4.0;
    
    if ( widthPan < originalLocation.x  && originalLocation.x< 3 *widthPan && heightPan <originalLocation.y && originalLocation.y < 3*heightPan) {
         NSLog(@"---000");
        [UIView animateWithDuration:0.1 animations:^{
            fitView.frame = CGRectMake(originX + x, originY + y, width, height);
        }];
    }else
    {
    
    if (originalLocation.x < width / 2.0 && originalLocation.y < height / 2.0) {
        NSLog(@"---111");
            [UIView animateWithDuration:0.1 animations:^{
                fitView.frame= CGRectMake(originX + x, originY + y, width-x , height-y );
            }];
        
    }else if (originalLocation.x > width / 2.0 && originalLocation.y < height / 2.0){
         NSLog(@"---222");
        [UIView animateWithDuration:0.1 animations:^{
            fitView.frame = CGRectMake(originX, originY + y, width +  x, height - y);
        }];
    }else if (originalLocation.x < width / 2.0 && originalLocation.y > height / 2.0){
         NSLog(@"---333");
        [UIView animateWithDuration:0.1 animations:^{
            fitView.frame = CGRectMake(originX + x, originY , width - x, height  + y);
            }];
    }
    else if(originalLocation.x > width / 2.0 && originalLocation.y > height / 2.0){
         NSLog(@"---444");
        [UIView animateWithDuration:0.3 animations:^{
            fitView.frame= CGRectMake(originX, originY, width + x, height + y);
        }];
    }
    
    }
   
}
