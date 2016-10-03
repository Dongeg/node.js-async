# node.js-async
async 流程控制

直接上个实例吧

在express+ejs+mongoose中，如果要想一个页面返回多个集合，可以这样做：

1.
```js
index_image.find(function(errA, recordsA){
   index_video.find(function(errB, recordsB){
   	   console.log(recordsA)
   	   console.log(recordsB)
       res.render("index", {
         indeximage: recordsA,
         indexvideo: recordsB
       });
   });
});
	      

```




或者采用async
```js
.....其他依赖

var async = require('async');

.....

router.get('/', function(req, res, next) {
    //parallel(tasks, [callback]) （多个函数并行执行）
    async.parallel([
    	function(callback){
			index_image.find(function(err,data1){
				if(err){
					callback(err)
				}
		    else{
		    	callback(null, data1)
		    	console.log(1)
		    }
			})    		
    	},
   		function(callback){
			index_video.find(function(err,data2){
				if(err){
					callback(err)
				}
		    else{
		    	 callback(null, data2)
		    	 console.log(2)
		    }
			})
   		}

    ],function(err,result){
		if(err){
			 console.log(err);
		}
		else{
		  res.render('index', {
		  	indeximage:result[0],
		    indexvideo:result[1]
		    
	      })	
		}	 			
   	})
```
