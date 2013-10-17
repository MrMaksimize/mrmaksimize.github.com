---
published: "true"
layout: default
title: whoami
image: /assets/images/user_profile.png
---

<div class="container about">
	<h1>Hello There, I’m <span class="about-bold">Maksim Pecherskiy!</span></h1>
	<span class="about-large">I’m a <span class="about-italic">web and mobile developer</span> and <span class="about-italic">architect</span>. <br> <img src=" {{ page.image }}" alt="Maksim Pecherskiy" class="about-portrait img-responsive posts-collate-img"></span>
	<span class="about-medium">I hold a B.S. in Information Systems from DePaul University and a B.S. in International Business from Linkoping University. I have over 5 years of experience in the dev business. </span> 
<span class = "about-small">I'm an avid traveler and enjoy learning and interacting with new cultures, meeting new people, and arguing viewpoints that differ from mine. I adapt quickly to new cultures and always seek to push my comfort limits. I speak Russian, English, a decent amount of Spanish and just a little bit of French (I really mean very little).

I also love animals. And so does my wonderful girlfriend Amy, who I have the pleasure of living with.

We have one one cool cat (Clifton), one fat cat (Kuma - aptly named after a burger joint), one crazy dog (Willow) and one chill dog (Sadie).
		I use this Blog to share my experiences and my knowledge with the world.</span>

	<div class="about-button">
	    <a class="btn btn-xlarge btn-tales-one" href="#contact">Drop Me A Line</a>
	</div>
    <hr>
</div>
<div class="container">
    <div class="row">
        <div class="col-md-6 col-md-offset-3 tales-superblock">
            <h2>Social</h2>
            <div class="social-icons clearfix">
                <a href="http://www.twitter.com/{{ site.author.twitter }}" target="_blank" class="social-icon color-one">
                        <div class="inner-circle" ></div>
                        <i class="icon-twitter"></i>
                </a>

                <a href="http://www.linkedin.com/in/{{ site.author.linkedin }}" target="_blank" class="social-icon color-two">
                        <div class="inner-circle" ></div>
                        <i class="icon-linkedin"></i>
                </a>    

                <a href="https://www.github.com/{{ site.author.github }}" target="_blank" class="social-icon color-three">
                        <div class="inner-circle" ></div>
                        <i class="icon-github-alt"></i>
                </a>
            </div>
            <hr>
        </div>
    </div>
    <div class="row">
        <div class="col-md-6 col-md-offset-3 tales-superblock" id="contact">
            <h2>Contact</h2>
            
            <form action="#" method="get" accept-charset="utf-8" class="contact-form">
                <input type="text" name="name" id="contact-name" placeholder="Name" class="form-control input-lg">
                <input type="email" name="email" id="contact-email" placeholder="Email" class="form-control input-lg">
                <textarea rows="10" name="message" id="contact-body" placeholder="Your thoughts..." class="form-control input-lg"></textarea>
                <div class="buttons clearfix">
                    <button type="submit" class="btn btn-xlarge btn-tales-one">Submit</button>
                </div>                    
            </form>
        </div>
    </div>        
</div>



