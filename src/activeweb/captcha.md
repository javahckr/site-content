<ol class=breadcrumb>
   <li><a href=/>JavaLite</a></li>
   <li><a href=/activeweb>ActiveWeb</a></li>
   <li class=active>Captcha</li>
</ol>
<div class=page-header>
   <h1>Captcha <small></small></h1>
</div>

[Captcha](http://en.wikipedia.org/wiki/CAPTCHA) is often used in dynamic websites to protect from bots. ActiveWeb
has Captcha built-in and does not depend on any third party library for it.

## Usage


Here is an example of a CaptchaController:

~~~~ {.java  }
public class CaptchaController extends AppController {
    public void index() throws IOException {
        String captchaText = Captcha.generateText();
        session("captcha", captchaText);
        outputStream("image/png").write(Captcha.generateImage(captchaText));
    }
}
~~~~

This controller has only three lines of code.

-   Line 3. it generates some random text using a class Captcha
-   Line 4. the text is placed into a session
-   Line 5. the Captcha class is used to generate a PNG image and stream it to the browser. This image will have
the same text as as what is stored in the current session, and once the form is submitted, you can compare
(in a different controller) text that is submitted in a form and text found in the session.

## Rendering Captcha

~~~~ {.html}
<img class="captcha" src="${context_path}/captcha" id="captcha_img">
<a href="#" onclick="$('#captcha_img').attr('src', '${context_path}/captcha?t=' + new Date().getTime());">Reset captcha</a>
~~~~

Rendering Captcha is done by simply pointing an IMG tag to the `CaptchaController.index()` method. Each time the
action is executed, a new text and new image is generated.

The "Reset captcha" link is an example on how to re-generate the text and image. The timestamp is needed sometimes
to prevent the browser from caching the image.

## Generated image


Here is the default image generated by the framework:

![Captcha image](images/captcha.png)

Code that generates it can be found here: [Captcha.java](https://github.com/javalite/activeweb/blob/master/activeweb/src/main/java/org/javalite/activeweb/Captcha.java).
This class can be easily modified to make the Captcha harder or easier to read.

An example of using Captcha can be found in the [Kitchensink](https://github.com/javalite/kitchensink/blob/master/src/main/java/app/controllers/CaptchaController.java) sample application.
