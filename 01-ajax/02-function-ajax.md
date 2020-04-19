~~~js
function myajax(json){
    let xhr = new XMLHttpRequset()
    let data = json.data
    function join(data){
		let str = "?"
		for(let i in data){
			str += i + "=" + data[i] + "&" 
		}
		// return str.replace(/&$/,"")
		// ?name=midi&age=20
		return str.slice( 0 , -1)
	}
    if(json.type.toLowerCase() === "get"){
		xhr.open( "get" , "http://localhost:8000" + join(json.data) , true)
        xhr.send( null )
    }else{
       xhr.open( "POST" , "http://localhost:8000" , true) 
        xhr.send( join(data) )
    }
    xhr.onreadystatechange = function(){
		//判断readyState状态码 和 http状态码
		if( xhr.readyState == 4 && xhr.status == 200 ){
			//xhr.responseText 是后台返回的数据
			document.body.innerHTML  = xhr.responseText
		}
	}
}
myajax({
    url: "http://localhost:8000",
    type: "get",
    data: {
        a1:"a1",
        a2:"a2"
    },
    success: function(data){
        console.log(data)
	}
})
~~~

