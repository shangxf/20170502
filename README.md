# 20170502
function json2url(json){
   //防止缓存;
   json.t = Math.random();
   var arr = [];
   for(var name in json){
      arr.push(name + '=' + json[name]);
   }
   return arr.join('&');
}

function ajax(json){
   //考虑默认情况;
   if(!json.url){
      alert('请填写正确的请求路径!');
      return;
   }
   json.time = json.time || 5000;
   json.type = json.type || 'get';
   json.data = json.data || {};

   if(window.XMLHttpRequest){
      var oAjax = new XMLHttpRequest();
   }else{
      var oAjax = new ActiveXObject('Microsoft.XMLHTTP');
   }

   switch (json.type.toLowerCase()){
      case 'get':
         oAjax.open('get',json.url+'?'+json2url(json.data),true);
         oAjax.send();
         break;
      case 'post':
         oAjax.open('post',json.url,true);
         oAjax.setRequestHeader('Content-type','application/x-www-form-urlencoded');
         oAjax.send(json2url(json.data));
   }

   oAjax.onreadystatechange = function(){
      if(oAjax.readyState == 4){
         if(oAjax.status >= 200 && oAjax.status < 300 || oAjax.status == 304){
            json.success && json.success(oAjax.responseText);
         }else{
            json.error && json.error(oAjax.status);
         }
         clearInterval(timer);
      }
   };

   var timer;

   timer = setTimeout(function(){
      alert('网络响应超时!');
      oAjax.onreadystatechange = null;
   },json.time);

}
