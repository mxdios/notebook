## lifeform项目记录

### for循环的进化

	for var i = 0; i < 30; ++i {
            
    }
    
这种形式的for循环会有警告提示

*‘++’ is deprecated: it will be removed in swift 3*

说++这种形式快过时了，swift3就删除了，赶紧换吧。 然后我改成这种形式的。

	for var i = 0; i < 30; i+=1 {
            
    }

然后就就如下提示

*C-style for statement is deprecated and will be removed in a future version of Swift*

c的风格是不被推荐的，在后续版本中还是会被删除的。OMG

点击警告，代码会被修改为推荐的风格，立马变成 for-in 风格，清新脱俗

	 for i in 0 ..< 30 {

     }