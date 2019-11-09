/**
  * Helper functions
  */
var getRandomInt = function(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

/**
  *  Fire the Pixel with the specified Pixel Id.
  */
var firePixel = function(pixelSrc) {
  var cb = Math.floor(Math.random() * 99999);
  var img = document.createElement('img');
  img.src = pixelSrc.replace('[[[CACHEBUSTER]]]', cb);
};

/* Call a Pixel service endpoint hosted in the UST Servlet application.
 *
 */
var callPixelService = function(endpoint, postParams) {

  var xmlhttp = new XMLHttpRequest();
  xmlhttp.open("POST", endpoint, true);
  xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  xmlhttp.send(postParams);
};


/* Call a Pixel service endpoint hosted in the UST Servlet application using an img tag
 *
 */
var callPixelServiceGet = function(endpoint, getParams) {

  var img = document.createElement('img');
  img.src = endpoint + '?' + getParams;
};

/* Return the query string value with the specified key, or "" if that
* isn't present in the URL.
* http://stackoverflow.com/questions/901115/how-can-i-get-query-string-values-in-javascript/901144#901144
*/
var getParameterByName = function(name) {
  name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
  var regex = new RegExp("[\\?&]" + name + "=([^&#]*)");
  results = regex.exec(location.search);
  return results === null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
};


var getHashParameter = function(name) {
  name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
  var regex = new RegExp("[\\?&]" + name + "=([^&#]*)");
  results = regex.exec(location.search.replace("%23",""));
  return results === null ? "" : results[1].replace(/\+/g, " ");
};

/* return the cookie with the specified name, or null if none exists. */
var getCookie = function(cname) {
  var name = cname + "=";
  var ca = document.cookie.split(';');
  for(var i = 0; i < ca.length; i++) {
    var c = ca[i];
    while (c.charAt(0) == ' ') {
      c = c.substring(1);
    }
    if (c.indexOf(name) === 0) {
      return c.substring(name.length, c.length);
    }
  }
  return null;
};

/*
* Set all the cookies specified in the query string.
* The query string key is 'set_cookies'
*/
var setCookies = function() {
  var cookieList = getParameterByName('set_cookies');
  if(!cookieList) {
    return; // avoid 1 elem array from splitting empty string.
  }

  // Cookies to be set are delimited by ','
  var cookieNames = cookieList.split(',');

  for(var i = 0; i < cookieNames.length; i++) {
    setCookieByName(cookieNames[i]);
  }
};

/** Record the frequency that a DX cookie has been placed. 
 *  The value starts at 1, and after it reaches a threshold, a special pixel is fired.
 *    freqPixelId: (string) The pixel id of the 'special' frequency pixel.
 *    threshold:   (int)    The frequency number after which the special pixel will be fired.  
 */
var updateFrequency = function(freqPixelId, threshold) {
  if( !freqPixelId )
    return;

  if( !threshold || isNaN(parseInt(threshold, 10)) )
    return;

  var dxFreqCookieName = "wfivefivecfreq";
  // Set or reset DX Frequency Cookie.
  var dxFreqCookie = getCookie(dxFreqCookieName);
  var dxFrequency = isNaN(parseInt(dxFreqCookie, 10)) ? 1 : parseInt(dxFreqCookie, 10) + 1;
  var cookieDate = new Date;
  cookieDate.setMonth(cookieDate.getMonth() + 1);
  document.cookie = dxFreqCookieName + "=" + dxFrequency + '; expires=' + cookieDate.toGMTString( ) + ";domain=.w55c.net;path=/";

  // If frequency is >= 15 fire frequency audience pixel
  if( dxFrequency >= parseInt(threshold, 10) ){
    var pixelURL = location.protocol + "//tags.w55c.net/rs?id=" + freqPixelId + "&rnd=[[[CACHEBUSTER]]]";
    firePixel(pixelURL);
  }
};

/*
* Set the cookie with the name specified.
* This page and script are hosted on cti.w55c.net
* Cookies are relative to this domain.
*/
var setCookieByName = function(cname) {
  if(!cname) {
    return;
  }

  var match = getCookie(cname);
  if(match !== null) {
    return;
  }

  if( cname === "wfivefivec" ){
    setDxCookie();
  }else{
    cookie_date = new Date;
    cookie_time = cookie_date.getTime();
    cookie_time += 3600 * 1000;
    cookie_date.setTime(cookie_time);
    document.cookie = cname + '=1; expires=' + cookie_date.toGMTString( ) + ';domain=.w55c.net;path=/';
  }
};

/* Set the dataxu "wfivefivec" cookie.
 * return the newly generated cookie value
 */
var setDxCookie = function() {
  var dxCookieName = "wfivefivec";
  var dxCookie = guid();
  var cookieDate = new Date;
  cookieDate.setFullYear(cookieDate.getFullYear( ) + 1);
  document.cookie = dxCookieName + '=' + dxCookie + '; expires=' + cookieDate.toGMTString( ) + ";domain=.w55c.net;path=/";

  return dxCookie;
};

var guid = (function() {
  Number.prototype.toBase = function(base) {
    var symbols =
    "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
    var decimal = this;
    var conversion = "";

    if (base > symbols.length || base <= 1) {
      return false;
    }

    while (decimal >= 1) {
      conversion = symbols[(decimal - (base * Math.floor(decimal / base)))] + conversion;
      decimal = Math.floor(decimal / base);
    }

    return (base < 11) ? parseInt(conversion) : conversion;
  }

  function pad(pad, str, padLeft) {
    if (typeof str === 'undefined') {
      return pad;
    }

    if (padLeft) {
      return (pad + str).slice(-pad.length);
    } else {
      return (str + pad).substring(0, pad.length);
    }
  }

  // For information about the construction of this UID, including why
  // the source is "6", see https://dataxu.atlassian.net/wiki/display/dev/Cookie+Sourcing
  return function() {
    MAX_RANDOM = 218340105584895;
    var d = new Date();
    var seconds = Math.round(d.getTime() / 1000);

    random_start = pad("00000000", getRandomInt(0, MAX_RANDOM).toBase(62), false);
    epoch_secs = pad("000000", seconds.toBase(62), false);
    source = "6";
    return random_start + epoch_secs + source;
  };
})();
