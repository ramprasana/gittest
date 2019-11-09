/*
  s_isValidEngagement
  
  @Description: 
  This function is for conditionally filtering engagement tracking triggered from  
  a s_trackEngagement call. 

  @param v44 (string) engagement label (does not have prefix e.g. "CLICK|". This 
     is a value that is passed to s_trackEngagement. 
  
  @return bool false to suppress, true to allow

  @Current Business Rules (first one wins):
  - allow all engagement tracking on all dev/qa sites
  - suppress (blacklist) the following from country sites
    - starts with "header"
    - starts with "footer"
    - starts with "nav_L"
  - all all engagement tracking on all prod sites

*/
function s_isValidEngagement(v44) {
  var p,l,m,isCountrySite,blackList;
  var v44=(v44)?String(v44):'';
  var rsid = (window.s_account||'').replace(/(^|,)sapglobal(,|$)/,'');


  /**
   Rule #1: Allow all if not prod
   **/
   if (
     (location.hostname.toLowerCase()!='go.sap.com')
      ||
     (location.hostname.toLowerCase()!='www.sap.com') 
   ) { 
     //console.log('DTM: s_isValidEngagement: engagement allowed: matched (dev site)');
     return true;
   }
  

  /**
   Rule #2: suppress (blacklist) the following from country sites
   **/
   isCountrySite = !rsid.match(/(^|,)sapcomglobal(,|$)/i);
   blackList = [
     {pattern:'^header:'},
     {pattern:'^footer:'},
     {pattern:'^nav_L[0-9]+:'}
   ];
   if (isCountrySite) {
     for (p=0,l=blackList.length;p<l;p++) {
       m = new RegExp(blackList[p].pattern,'i');
       if (v44.match(m)) {
         //console.log('DTM: s_isValidEngagement: engagement suppressed: matched (blacklist)');
         return false;
       }
     }
   }


  /**
   Rule #3: default allow on prod
   **/
   //console.log('DTM: s_isValidEngagement: engagement allowed: matched (default)');
   return true;

    
} // s_isValidEngagement
