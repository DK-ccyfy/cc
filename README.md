# 此图片来自微信公众平台未经允许不可引用
 
 在做一个微信项目时候遇到了这个问题，当时是后台抓取数据，前台请求接口后返回带样式的json数据，部分图片显示时会出现该问题。
经过检查发现图片本身没有问题，应该是请求微信端服务器返回的该图片。
因此想到可能是根据 document.referrer 来判断来源的。 在新开一个空白页面打开图片链接，发现图片能够正确打开。
于是想到将img嵌套到一个iframe中，这样它的请求地址就为‘’，这样能够绕开微信的防盗链校验。
试验后果然成功了，这也是试验了许多办法能够成功的，代码如下，写的有点糙 - - 。


function showImg(url,_node,num) {
    window.img = '<img  src=\'' + url + '?' + Math.random() + '\' />';
    $(_node).parent().append('<iframe class="iframe-content"  src="javascript:parent.img;" frameBorder="0" scrolling="no" style="max-width: 100%;"></iframe>');
    $(_node).remove();
    $('body').find('iframe').contents().find('body').css('margin','0');
    $('body').find('iframe').contents().find('img').css('max-width','100%');
    setTimeout(function(){
        var tar =$('body').find('iframe')[num-1];
        var _heihth = $(tar).contents().find('img').height()+'px';console.log(_heihth);
        $(tar).css('height',_heihth);
        $(tar).contents().find('img').css('height',_heihth);
    },1000)

};
setTimeout(function(){
    $('img').each(function(i){
        if ($(this).attr('data-src')) {
            setTimeout(function(){
                if ($(this).attr('data-src')) {
                    showImg($(this).attr('data-src'),$(this),i);
                }
            }.bind(this),500);
        }
    }) ;
}, 1000);

请注意务必将这段放在数据渲染之后

