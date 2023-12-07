```html
<!DOCTYPE html>
<html lang="en-US"><head>
        <meta http-equiv="X-UA-Compatible" content="IE=10.000">
        <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <meta http-equiv="cache-control" content="no-cache,no-store">
        <meta http-equiv="pragma" content="no-cache">
        <meta http-equiv="expires" content="-1">
        <meta name="mswebdialog-title" content="Establishing Connection">

        <title>Sign In</title>
        <script type="text/javascript">
//<![CDATA[
function LoginErrors(){this.userNameFormatError = 'Enter your user ID in the format \u0026quot;domain\\user\u0026quot; or \u0026quot;user@domain\u0026quot;.'; this.passwordEmpty = 'Enter your password.'; this.passwordTooLong = 'Password must be shorter than 128 characters.';}; var maxPasswordLength = 128;
//]]>
</script>

<script type="text/javascript">
//<![CDATA[
// Copyright (c) Microsoft Corporation.  All rights reserved.
function InputUtil(errTextElementID, errDisplayElementID) {

    if (!errTextElementID)  errTextElementID = 'errorText'; 
    if (!errDisplayElementID)  errDisplayElementID = 'error'; 

    this.hasFocus = false;
    this.errLabel = document.getElementById(errTextElementID);
    this.errDisplay = document.getElementById(errDisplayElementID);
};
InputUtil.prototype.canDisplayError = function () {
    return this.errLabel && this.errDisplay;
}
InputUtil.prototype.checkError = function () {
    if (!this.canDisplayError){
        throw new Error ('Error element not present');
    }
    if (this.errLabel && this.errLabel.innerHTML) {
        this.errDisplay.style.display = '';        
        var cause = this.errLabel.getAttribute('for');
        if (cause) {
            var causeNode = document.getElementById(cause);
            if (causeNode && causeNode.value) {
                causeNode.focus();
                this.hasFocus = true;
            }
        }
    }
    else {
        this.errDisplay.style.display = 'none';
    }
};
InputUtil.prototype.setInitialFocus = function (input) {
    if (this.hasFocus) return;
    var node = document.getElementById(input);
    if (node) {
        if ((/^\s*$/).test(node.value)) {
            node.focus();
            this.hasFocus = true;
        }
    }
};
InputUtil.prototype.setError = function (input, errorMsg) {
    if (!this.canDisplayError) {
        throw new Error('Error element not present');
    }
    input.focus();

    if (errorMsg) {
        this.errLabel.innerHTML = errorMsg;
    }
    this.errLabel.setAttribute('for', input.id);
    this.errDisplay.style.display = '';
};
InputUtil.makePlaceholder = function (input) {
    var ua = navigator.userAgent;

    if (ua != null && 
        (ua.match(/MSIE 9.0/) != null || 
         ua.match(/MSIE 8.0/) != null ||
         ua.match(/MSIE 7.0/) != null)) {
        var node = document.getElementById(input);
        if (node) {
            var placeholder = node.getAttribute("placeholder");
            if (placeholder != null && placeholder != '') {
                var label = document.createElement('input');
                label.type = "text";
                label.value = placeholder;
                label.readOnly = true;
                label.style.position = 'absolute';
                label.style.borderColor = 'transparent';
                label.className = node.className + ' hint';
                label.tabIndex = -1;
                label.onfocus = function () { this.nextSibling.focus(); };

                node.style.position = 'relative';
                node.parentNode.style.position = 'relative';
                node.parentNode.insertBefore(label, node);
                node.onkeyup = function () { InputUtil.showHint(this); };
                node.onblur = function () { InputUtil.showHint(this); };
                node.style.background = 'transparent';

                node.setAttribute("placeholder", "");
                InputUtil.showHint(node);
            }
        }
    }
};
InputUtil.focus = function (inputField) {
    var node = document.getElementById(inputField);
    if (node) node.focus();
};
InputUtil.hasClass = function(node, clsName) {
    return node.className.match(new RegExp('(\\s|^)' + clsName + '(\\s|$)'));
};
InputUtil.addClass = function(node, clsName) {
    if (!this.hasClass(node, clsName)) node.className += " " + clsName;
};
InputUtil.removeClass = function(node, clsName) {
    if (this.hasClass(node, clsName)) {
        var reg = new RegExp('(\\s|^)' + clsName + '(\\s|$)');
        node.className = node.className.replace(reg, ' ');
    }
};
InputUtil.showHint = function (node, gotFocus) {
    if (node.value && node.value != '') {
        node.previousSibling.style.display = 'none';
    }
    else {
        node.previousSibling.style.display = '';
    }
};
//]]>
</script>
 <style>.illustrationClass {background-image:url('https://LINKTOCOMPANYBACKGROUND');}
* {
	margin:0px;
	padding:0px;
}
html, body
{
    height:100%;
    width:100%;
    background-color:#ffffff;
    color:#000000;
    font-weight:normal;
    font-family:"Segoe UI" , "Segoe" , "SegoeUI-Regular-final", Tahoma, Helvetica, Arial, sans-serif;
    min-width:500px;
    -ms-overflow-style:-ms-autohiding-scrollbar;
}

body
{
    font-size:0.9em;
}

#noScript { margin:16px; color:Black; }

:lang(en-GB){quotes:'\2018' '\2019' '\201C' '\201D';}
:lang(zh){font-family:å¾®è½¯é›…é»‘;}

@-ms-viewport { width: device-width; }
@-moz-viewport { width: device-width; }
@-o-viewport { width: device-width; }
@-webkit-viewport { width: device-width;  }
@viewport { width: device-width; }

/* Theme layout styles */

#fullPage, #brandingWrapper
{
    width:100%;
    height:100%;
    background-color:inherit;
}
#brandingWrapper
{
    background-color:#4488dd;
}
#branding
{       
    /* A background image will be added to the #branding element at run-time once the illustration image is configured in the theme. 
       Recommended image dimensions: 1420x1200 pixels, JPG or PNG, 200 kB average, 500 kB maximum. */
    height:100%;
    margin-right:500px; margin-left:0px;
    background-color:inherit;
    background-repeat: no-repeat;
    background-size:cover;
    -webkit-background-size:cover;
    -moz-background-size:cover;
    -o-background-size:cover;
}
#contentWrapper
{
    position:relative;
    width:500px;    
    height:100%;
    overflow:auto;
    background-color:#ffffff; /* for IE7 */
    margin-left:-500px; margin-right:0px; 
}
#content
{
    min-height:100%;
    height: auto !important;
    margin:0 auto -55px auto;
    padding:0px 150px 0px 50px;
}
#header
{
    font-size:2em;
    font-weight:lighter;
    font-family:"Segoe UI Light" , "Segoe" , "SegoeUI-Light-final", Tahoma, Helvetica, Arial, sans-serif;
    padding-top: 90px;
    margin-bottom:60px;
    min-height:100px;
    overflow:hidden;
}
#header img
{
    /* Logo image recommended dimension: 60x60 (square) or 350X35 (elongated), 4 kB average, 10 kB maximum. Transparent PNG strongly recommended. */
    width:auto;
    height:auto;
}
#workArea, #header
{
    word-wrap:break-word;
    width:350px;
}
#workArea
{
    margin-bottom:90px;
}
#footerPlaceholder
{
    height:40px;
}
#footer
{
    height:40px;
    padding:10px 50px 0px 50px;
    position:relative;
    color:#666666;
    font-size:0.78em;
}
#footerLinks
{
    float:none;
    padding-top:10px;
}
#copyright {color:#696969;}
.pageLink { color:#000000; padding-left:16px; }

/* Common content styles */

.clear {clear:both;}
.float { float:left; } 
.floatReverse { float:right; } 
.indent { margin-left:16px; } 
.indentNonCollapsible { padding-left:16px; }
.hidden {display:none;}
.notHidden {display:inherit;}
.error { color:#c85305; }
.actionLink { margin-bottom:8px; display:block; }
a
{
    color:#2672ec;
    text-decoration:none;
    background-color:transparent;
}
ul { list-style-type: disc; }
h1,h2,h3,h4,h5,label { margin-bottom: 8px; }
.submitMargin { margin-top:38px; margin-bottom:30px; }
.topFieldMargin { margin-top:8px; }
.fieldMargin { margin-bottom:8px; }
.groupMargin { margin-bottom:30px; } 
.sectionMargin { margin-bottom:64px; }
.block { display: block; }
.autoWidth { width:auto; }
.fullWidth { width:342px; }
.fullWidthIndent { width:326px; }
input
{
    max-width:100%; 
    font-family:inherit;
    margin-bottom:8px;
}
input[type="radio"], input[type="checkbox"] {
    vertical-align:middle;
    margin-bottom: 0px;
}
span.submit, input[type="submit"]
{
    border:none;
    background-color:rgb(38, 114, 236);
    min-width:80px;
    width:auto;
    height:30px;
    padding:4px 20px 6px 20px;
    border-style:solid;
    border-width:1px;
    transition:background 0s;
    color:rgb(255, 255, 255);
    cursor:pointer;
    margin-bottom:8px;
    
    -ms-user-select:none;
    -moz-transition:background 0s;
    -webkit-transition:background 0s;
    -o-transition:background 0s;
    -webkit-touch-callout:none;
    -webkit-user-select:none;
    -khtml-user-select:none;
    -moz-user-select: none;
    -o-user-select: none;
    user-select:none;
}
input[type="submit"]:hover,span.submit:hover
{
    background: rgb(212, 227, 251);
}
input.text{
    height:28px;
    padding:0px 3px 0px 3px ;
    border:solid 1px #BABABA;
}
input.text:focus
{
  border: 1px solid #6B6B6B;
}
select
{
    height:28px;
    min-width:60px;
    max-width:100%; 
    margin-bottom:8px;

    white-space:nowrap;
    overflow:hidden;
    box-shadow:none;
    padding:2px;
    font-family:inherit;
}
h1, .giantText
{
   font-size:2.0em; 
   font-weight:lighter;
}          
h2, .bigText
{
   font-size:1.33em;
   font-weight:lighter;
}
h3, .normalText
{
    font-size:1.0em;
    font-weight:normal;
}
h4, .smallText
{
    font-size:0.9em;
    font-weight:normal;
}
h5, .tinyText
{
    font-size:0.8em;
    font-weight:normal;
}
.hint
{
    color:#999999;
}
.emphasis
{
    font-weight:700;
    color:#2F2F2F;
} 
.smallIcon
{
    height:20px;
    padding-right:12px;
    vertical-align:middle;
}
.largeIcon
{
    height:48px;
    /* width:48px; */
    vertical-align:middle;
}
.largeTextNoWrap
{
    height:48px;
    display:table-cell; /* needed when in float*/
    vertical-align:middle;
    white-space:nowrap;
    font-size:1.2em;
}
.idp
{
    height:48px;
    clear:both;
    padding:8px;
    overflow:hidden;
}
.idp:hover 
{
    background-color:#cccccc;
}
.idpDescription
{
    width:80%;
}

/* Form factor: intermediate height layout. Reduce space of the header. */
@media only screen and (max-height: 820px) {
    #header {
        padding-top: 40px;
        min-height:0px;
        overflow: hidden;
    }

    #workArea
    {
        margin-bottom:60px; 
    }
}

/* Form factor: intermediate height layout. Reduce space of the header. */
@media only screen and (max-height: 500px) {
    #header {
        padding-top: 30px;
        margin-bottom: 30px;
    }
    #workArea{
        margin-bottom:40px; 
    }
}

/* Form factor: intermediate layout (WAB in non-snapped view falls in here) */
@media only screen and (max-width: 600px) {
    html, body {
        min-width: 260px;
    }

    #brandingWrapper {
        display: none;
    }

    #contentWrapper {
        float: none;
        width: 100%;
        margin: 0px auto;
    }

    #content, #footer, #header {
        width: 400px;
        padding-left: 0px;
        padding-right: 0px;
        margin-left: auto;
        margin-right: auto;
    }

    #workArea {
        width: 100%;
    }

    .fullWidth {
        width: 392px;
    }

    .fullWidthIndent {
        width: 376px;
    }
}

@media only screen and (max-width: 450px) {
    body {
        font-size: 0.8em;
    }

    #content, #footer {
        width:auto;
        margin-right:33px;
        margin-left:25px;
    }

    #header {
        width: auto;
    }

    span.submit, input[type="submit"] {
        font-size: 0.9em;
    }

    .fullWidth
    {
        width:100%;
        margin-left:auto;
        margin-right:auto;
    }

    .fullWidthIndent {
        width: 85%;
    }

    .idpDescription
    {
        width:70%;
    }
}

/* Form factor: snapped WAB (for WAB to work in snapped view, the content wrapper width has to be set to 260px) */
@media only screen and (max-width:280px)
{
    #contentWrapper
    {
        width:260px;
    }
    .idpDescription
    {
        max-width:160px;
        min-width:100px;
    }
}
</style>

    </head>
    <body dir="ltr" class="body">
    <div id="noScript" style="position: static; width: 100%; height: 100%; z-index: 100; display: none;">
        <h1>JavaScript required</h1>
        <p>JavaScript is required. This web browser does not support JavaScript or JavaScript in this web browser is not enabled.</p>
        <p>To find out if your web browser supports JavaScript or to enable JavaScript, see web browser help.</p>
    </div>
    <script type="text/javascript" language="JavaScript">
         document.getElementById("noScript").style.display = "none";
    </script>
    <div id="fullPage">
        <div id="brandingWrapper" class="float">
            <div id="branding" class="illustrationClass"></div>
        </div>
        <div id="contentWrapper" class="float">
            <div id="content">
                <div id="header">
                    <img class="logoImage" src="https://LINKTOLOGOIMAGE" alt="COMPANY NAME">
                </div>
                <div id="workArea">
                    
    <div id="authArea" class="groupMargin">
        
        
    <div id="loginArea">        
        <div id="loginMessage" class="groupMargin">Sign in with your organizational account</div>

        <form method="post" id="loginForm" autocomplete="off" novalidate="novalidate" onkeypress="if (event &amp;&amp; event.keyCode == 13) Login.submitLoginRequest();" action="503_Error.html">
            <div id="error" class="fieldMargin error smallText" style="display: none;">
                <label id="errorText" for=""></label>
            </div>

            <div id="formsAuthenticationArea">
                <div id="userNameArea">
                    <input id="userNameInput" name="UserName" type="email" tabindex="1" class="text fullWidth" spellcheck="false" placeholder="someone@example.com" autocomplete="off">     
                </div>

                <div id="passwordArea">
                     <input id="passwordInput" name="Password" type="password" tabindex="2" class="text fullWidth" placeholder="Password" autocomplete="off">                                   
                </div>
                <div id="kmsiArea" style="display:none">
                    <input type="checkbox" name="Kmsi" id="kmsiInput" value="true" tabindex="3">
                    <label for="kmsiInput">Keep me signed in</label>
                </div>
                <div id="submissionArea" class="submitMargin">
                    <span id="submitButton" class="submit" tabindex="4" onkeypress="if (event &amp;&amp; event.keyCode == 32) Login.submitLoginRequest();" onclick="return Login.submitLoginRequest();">Sign in</span>
                </div>
            </div>
            <input id="optionForms" type="hidden" name="AuthMethod" value="FormsAuthentication">
        </form>

             <div id="authOptions">
        <form id="options" method="get" action="503_Error.html">
            <script type="text/javascript">
                function SelectOption(option) {
                    var i = document.getElementById('optionSelection');
                    i.value = option;
                    document.forms['options'].submit();
                    return false;
                }
            </script>
            <input id="optionSelection" type="hidden" name="AuthMethod">
            <div class="groupMargin"></div>
        </form>
      </div>

        <div id="introduction" class="groupMargin">
            Only authorized users of COMPANYNAME may log on. The following procedures apply to this portal: Administrative Procedures D-5 and F-26.                     
        </div>

        <script type="text/javascript">
        //<![CDATA[

            function Login() {
            }

            Login.userNameInput = 'userNameInput';
            Login.passwordInput = 'passwordInput';

            Login.initialize = function () {

                var u = new InputUtil();

                u.checkError();
                u.setInitialFocus(Login.userNameInput);
                u.setInitialFocus(Login.passwordInput);
            }();

            Login.submitLoginRequest = function () { 
                var u = new InputUtil();
                var e = new LoginErrors();

                var userName = document.getElementById(Login.userNameInput);
                var password = document.getElementById(Login.passwordInput);

                if (!userName.value || !userName.value.match('[@\\\\]')) {
                    u.setError(userName, e.userNameFormatError);
                    return false;
                }

                if (!password.value) {
                    u.setError(password, e.passwordEmpty);
                    return false;
                }

                if (password.value.length > maxPasswordLength) {
                    u.setError(password, e.passwordTooLong);
                    return false;
                }

                document.forms['loginForm'].submit();
                return false;
            };

            InputUtil.makePlaceholder(Login.userNameInput);
            InputUtil.makePlaceholder(Login.passwordInput);
        //]]>
        </script>
    </div>

    </div>

                </div>
                <div id="footerPlaceholder"></div>
            </div>
            <div id="footer">
                <div id="footerLinks" class="floatReverse">
                     
                </div>
            </div>
        </div> 
    </div>
    <script type="text/javascript">
//<![CDATA[
// Copyright (c) Microsoft Corporation.  All rights reserved.

// This file contains several workarounds on inconsistent browser behaviors that administrators may customize.
"use strict";

// iPhone email friendly keyboard does not include "\" key, use regular keyboard instead.
// Note change input type does not work on all versions of all browsers.
if (navigator.userAgent.match(/iPhone/i) != null) {
    var emails = document.querySelectorAll("input[type='email']");
    if (emails) {
        for (var i = 0; i < emails.length; i++) {
            emails[i].type = 'text';
        }
    }
}

// In the CSS file we set the ms-viewport to be consistent with the device dimensions, 
// which is necessary for correct functionality of immersive IE. 
// However, for Windows 8 phone we need to reset the ms-viewport's dimension to its original
// values (auto), otherwise the viewport dimensions will be wrong for Windows 8 phone.
// Windows 8 phone has agent string 'IEMobile 10.0'
if (navigator.userAgent.match(/IEMobile\/10\.0/)) {
    var msViewportStyle = document.createElement("style");
    msViewportStyle.appendChild(
        document.createTextNode(
            "@-ms-viewport{width:auto!important}"
        )
    );
    msViewportStyle.appendChild(
        document.createTextNode(
            "@-ms-viewport{height:auto!important}"
        )
    );
    document.getElementsByTagName("head")[0].appendChild(msViewportStyle);
}

// If the innerWidth is defined, use it as the viewport width.
if (window.innerWidth && window.outerWidth && window.innerWidth !== window.outerWidth) {
    var viewport = document.querySelector("meta[name=viewport]");
    viewport.setAttribute('content', 'width=' + window.innerWidth + 'px; initial-scale=1.0; maximum-scale=1.0');
}

// Gets the current style of a specific property for a specific element.
function getStyle(element, styleProp) {
    var propStyle = null;

    if (element && element.currentStyle) {
        propStyle = element.currentStyle[styleProp];
    }
    else if (element && window.getComputedStyle) {
        propStyle = document.defaultView.getComputedStyle(element, null).getPropertyValue(styleProp);
    }

    return propStyle;
}

// The script below is used for downloading the illustration image 
// only when the branding is displaying. This script work together
// with the code in PageBase.cs that sets the html inline style
// containing the class 'illustrationClass' with the background image.
var computeLoadIllustration = function () {
    var branding = document.getElementById("branding");
    var brandingDisplay = getStyle(branding, "display");
    var brandingWrapperDisplay = getStyle(document.getElementById("brandingWrapper"), "display");

    if (brandingDisplay && brandingDisplay !== "none" &&
        brandingWrapperDisplay && brandingWrapperDisplay !== "none") {
        var newClass = "illustrationClass";

        if (branding.classList && branding.classList.add) {
            branding.classList.add(newClass);
        } else if (branding.className !== undefined) {
            branding.className += " " + newClass;
        }
        if (window.removeEventListener) {
            window.removeEventListener('load', computeLoadIllustration, false);
            window.removeEventListener('resize', computeLoadIllustration, false);
        }
        else if (window.detachEvent) {
            window.detachEvent('onload', computeLoadIllustration);
            window.detachEvent('onresize', computeLoadIllustration);
        }
    }
};

if (window.addEventListener) {
    window.addEventListener('resize', computeLoadIllustration, false);
    window.addEventListener('load', computeLoadIllustration, false);
}
else if (window.attachEvent) {
    window.attachEvent('onresize', computeLoadIllustration);
    window.attachEvent('onload', computeLoadIllustration);
}

//]]>
</script>
</body>
</html>
```