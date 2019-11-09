var SAP=SAP||{};
function addCookie(b,f,c){var e=new Date();
e.setTime(e.getTime()+(c*24*60*60*1000));
var a="expires="+e.toGMTString();
document.cookie=b+"="+f+"; "+a+"; path=/;"
}function getCookie(b){var c,a,e,d=document.cookie.split(";");
for(c=0;
c<d.length;
c++){a=d[c].substr(0,d[c].indexOf("="));
e=d[c].substr(d[c].indexOf("=")+1);
a=a.replace(/^\s+|\s+$/g,"");
if(a===b){return unescape(e)
}}}function deleteCookie(a){addCookie(a,"",-1)
}function cleanCookie(a){if(getCookie(a)){deleteCookie(a)
}}SAP.addJsonObjectToSessionStorage=function(c,a){try{if(typeof(Storage)!=="undefined"&&sessionStorage){sessionStorage.setItem(a,JSON.stringify(c))
}}catch(b){console.log("Error while adding object to session storage: "+b)
}};
SAP.getJsonObjectFromSessionStorage=function(a){var c=null;
if(typeof(Storage)!=="undefined"&&sessionStorage){try{c=JSON.parse(sessionStorage.getItem(a))
}catch(b){console.log("Error parsing json: "+b)
}}return c
};
SAP.detectCountryByIp=function(h){var c="country",b=SAP.getJsonObjectFromSessionStorage(c),f=document.getElementsByTagName("html")[0].getAttribute("data-language"),g="{}",a="/bin/sapdx/countries.fetchDefaultUserLocation.txt?pageLanguage="+f,d;
if((b===null||!b.proposeData||b.proposeData.translationLocale!==f)&&window.XMLHttpRequest){d=new XMLHttpRequest();
d.onreadystatechange=function(){if(d.readyState===XMLHttpRequest.DONE){if(d.status===200){if(d.responseText&&d.responseText!=="null"){g=d.responseText;
b=JSON.parse(g);
SAP.addJsonObjectToSessionStorage(b,c);
e(h,b)
}else{console.log("Can't determine current location. Data from response is incorrect.");
e(h,b)
}}else{console.error("Error with fetchDefaultUserLocation request : "+d.statusText);
e(h,b)
}}};
d.open("GET",a,true);
d.send()
}else{e(h,b)
}function e(j,i){if(typeof j==="function"){j(i)
}}};