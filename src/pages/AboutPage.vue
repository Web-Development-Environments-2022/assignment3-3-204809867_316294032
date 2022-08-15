<template>
  <div class="container">
    <center>
    <br>
    <br>
      <span style="
              text-align: center;
              font-size: 25px;
              letter-spacing: 7px;
              font-family: system-ui;
              height: 100%;
              width: 100%;
              background-position: center;
              background-size: cover;

              display: flex;
              justify-content: center;
              padding-bottom: 20px;
              padding-top: 10px;
              text-align: center;
            " >ABOUT US</span>
          <center>
				<img id="dar_ido" src="../assets/Dar-Ido2.jpeg" style="width: 250px;
			height: 380px;
      width: 240px;
			margin-bottom: -40px;
			margin-top: 5px;" />
			</center>
      <br>
      <br>
      <br>
    This project was made by Dar Abu and Ido Hersko as part of the Web Developement course in the Ben-Gurion University of the Negev.
    We are two friends who loves to code and debug ( who doesnt after all ?) and we are very happy to let you use this website.
    <br>
    <br>
    <br>
    <br>
    <span style="
              text-align: center;
              font-size: 25px;
              letter-spacing: 7px;
              font-family: system-ui;
            " > TECHNOLOGY</span>
    <br>
    We used in the backend node.js and express. In the front we used Vue and some other features like bootsrap.
    <br>
    <br>
    <br>
    <br>
    <span style="
              text-align: center;
              font-size: 25px;
              letter-spacing: 7px;
              font-family: system-ui;
            "> MORE FROM DAR & IDO</span>
    <br>
    <a href="https://github.com/Web-Development-Environments-2022/assignment3_2-204809867_316294032"> Server side of this project</a>
    <br>
    <a href="https://web-development-environments-2022.github.io/316294032_204809867/"> Pacman Game</a>
    <br>

    <a href="https://web-development-environments-2022.github.io/316294032/">Personal page of Olga Abu (Dar)</a>
    <br>

    <a href="https://web-development-environments-2021.github.io/assignment1-204809867/">Personal page of David Ben-Gurion (Ido)</a>
  </center>
<br>

    <footer>
    <div class=footer-container style="background-color:rgb(226, 226, 150);
;">
      <div class="footer-cointainer-child dar">
        <div class="footer-social-icons">
          <h4 class="_14">Dar Ab</h4>
          <ul class="social-icons">
            <li><a href="https://www.linkedin.com/in/dar-abu-53a581237/" class="social-icon"> <i
                  class="fa fa-linkedin"></i></a></li>
            <li><a href="https://github.com/Darabu273" class="social-icon"> <i
                  class="fa fa-github"></i></a>
            </li>
            <li><a href="mailto:darabu@post.bgu.ac.il" class="social-icon"> <i
                  class="fa fa-google"></i></a>
            </li>
            <li><a href="https://www.facebook.com/dar.abu.3" class="social-icon"> <i
                  class="fa fa-facebook"></i></a></li>
            <li><a href="https://www.instagram.com/dar_abu/" class="social-icon"> <i
                  class="fa fa-instagram"></i></a></li>
          </ul>
        </div>
      </div>
      <div class="footer-cointainer-child ido">
        <div class="footer-social-icons">
          <h4 class="_14">Ido Hersk</h4>
          <ul class="social-icons">
            <li><a href="https://www.linkedin.com/in/ido-hersko/" class="social-icon"> <i
                  class="fa fa-linkedin"></i></a></li>
            <li><a href="https://github.com/idohersko" class="social-icon"> <i
                  class="fa fa-github"></i></a>
            </li>
            <li><a href="mailto:hersko@post.bgu.ac.il" class="social-icon"> <i
                  class="fa fa-google"></i></a>
            </li>
            <li><a href="https://www.facebook.com/ido.hersko/" class="social-icon"> <i
                  class="fa fa-facebook"></i></a></li>
            <li><a href="https://www.instagram.com/ido_hersko/" class="social-icon"> <i
                  class="fa fa-instagram"></i></a></li>
          </ul>
        </div>
      </div>
      <div class="footer-cointainer-child ido">
        <div class="footer-social-icons">
          <h4 class="_14">About the project</h4>
          <span style="color: rgb(119, 119, 119);font-size: 13px;vertical-align:2px;">
            © 2022, This project was made in the scope of the Web Development course of Prof.
            Erez Shalom and Mr.David Zeltser in the Ben-Gurion University, Israel </span>
        </div>
      </div>

    </div>
  </footer> 



  </div>

</template>


<style scoped>
@import url(https://fonts.googleapis.com/css?family=Lato);
@import url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.6.3/css/font-awesome.css);

/* over on social-icons  */

.container{
   background-color: rgb(226, 226, 150);
  

}

li > a:hover{
    color: #d9d9d9;
    text-decoration:  underline;
}

.footer-social-icons {
    width: 350px;
    display:block;
    margin: 0 auto;

}

ul.social-icons {
    margin-top: 10px;
    margin-bottom: 10px;
}
.social-icons li {
    display: inline;

}
.social-icons a {
    color: #fff;
}
.fa-facebook {
    padding:10px 14px;
    -o-transition:.5s;
    -ms-transition:.5s;
    -moz-transition:.5s;
    -webkit-transition:.5s;
    transition: .5s;
    background-color: #322f30;
    border-radius: 50%;

}
.fa-facebook:hover {
    background-color: #1a62f3;
}
.fa-linkedin {
    padding:10px 14px;
    -o-transition:.5s;
    -ms-transition:.5s;
    -moz-transition:.5s;
    -webkit-transition:.5s;
    transition: .5s;
    background-color: #322f30;
    border-radius: 50%;
}
.fa-linkedin:hover {
    background-color: #e4467b;
}
.fa-github {
    padding:10px 14px;
    -o-transition:.5s;
    -ms-transition:.5s;
    -moz-transition:.5s;
    -webkit-transition:.5s;
    transition: .5s;
    background-color: #322f30;
    border-radius: 50%;
}
.fa-github:hover {
    background-color: #f6b910;
}
.fa-instagram {
    padding:10px 14px;
    -o-transition:.5s;
    -ms-transition:.5s;
    -moz-transition:.5s;
    -webkit-transition:.5s;
    transition: .5s;
    background-color: #322f30;
    border-radius: 50%;
}
.fa-instagram:hover {
    background-color: #69914d;
}

.fa-google {
    padding:10px 14px;
    -o-transition:.5s;
    -ms-transition:.5s;
    -moz-transition:.5s;
    -webkit-transition:.5s;
    transition: .5s;
    background-color: #322f30;
    border-radius: 50%;
}
.fa-google:hover {
    background-color: #5a32a3;
}

/*now the container div seting to put the dar div and ido div side by side*/

.footer-container {
    display: flex;

    display: flex;
    justify-content: center;
    align-items: center;
    height: 73px;
    padding: 80px ;
    font-size: 15px;
    color: #f79839;
    

}


.footer-container-child:first-child {
    margin-right: 20px;
} 

._14 {
    text-align: center;
    
}

</style>
