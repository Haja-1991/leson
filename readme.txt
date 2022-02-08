Formulaire vers courriel basique avec contrôle captcha anti spam (form to mail)-------------------------------------------------------------------------------
Url     : http://codes-sources.commentcamarche.net/source/52898-formulaire-vers-courriel-basique-avec-controle-captcha-anti-spam-form-to-mailAuteur  : cod57Date    : 05/08/2013
Licence :
=========

Ce document intitulé « Formulaire vers courriel basique avec contrôle captcha anti spam (form to mail) » issu de CommentCaMarche
(codes-sources.commentcamarche.net) est mis à disposition sous les termes de
la licence Creative Commons. Vous pouvez copier, modifier des copies de cette
source, dans les conditions fixées par la licence, tant que cette note
apparaît clairement.

Description :
=============

Bonjour
<br />
<br />Petite source qui est souvent trait&eacute;e sur le forum

<br />
<br />- Protection captcha basique
<br />- Contr&ocirc;le des champs 
en PHP et JS
<br />- Une fonction PHP de contr&ocirc;le de mail (la votre ou un
 autre snippet ...)
<br />- Une fonction PHP pour pas se faire poster des roman
s et un peu de protection xss
<br />- Protection contre le double-post, double 
validation (par $_SESSION).
<br />- Bouton de reload pour le captcha.
<br />- 
Contr&ocirc;le javascript des champs avec alert en plus du contr&ocirc;le php.

<br />
<br />Le code est &agrave; but p&eacute;dagogique pour les d&eacute;buta
nts qui cherchent une m&eacute;thode de traitement
<br />Ici un 'form To mail' 
mais cela peut &ecirc;tre un formulaire pour poster des annonces ...
<br />
<b
r />a++
<br /><a name='source-exemple'></a><h2> Source / Exemple : </h2>
<br 
/><pre class='code' data-mode='basic'>
&lt;?php
session_start();

function V
erifierAdresseMail($mail){
$Syntaxe='#^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,6}$#';
if(p
reg_match($Syntaxe,$mail)){
return true;
}else{
return false;
}
}

/* Pet
itClean($var,$lg) */
/* $var la varible à traiter */
/* la longueur de sortie 
*/  

function PetitClean($var,$lg){
$var=strip_tags($var);
  /* troncature 
on va pas me poster un roman (-: */
  if(strlen($var)&gt;$lg){
  $var = substr
($var, 0, $lg);
  $last_space = strrpos($var, &quot; &quot;);
  $var = substr(
$var, 0, $last_space);
  }else{
  $lg=0;
  } 
return $var;
}
    
$error=
NULL;

if(isset($_POST['nom']) &amp;&amp; !empty($_POST['nom'])){
$nom=$_POST
['nom'];$error=NULL;
//filtrage 
$nom=PetitClean($nom,30); /*30 caractères max
i*/
}else{
echo $error='&lt;h3 align=&quot;center&quot;&gt;Le nom est vide - &
lt;a href=&quot;javascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&g
t;&lt;/h3&gt;';exit;
}

if(isset($_POST['mail']) &amp;&amp; !empty($_POST['ma
il'])){
$mail=$_POST['mail'];$error=NULL;$mail=htmlentities($mail);
//filtrage

$mail=PetitClean($mail,60);
}else{
echo $error='&lt;h3 align=&quot;center&qu
ot;&gt;Le e-mail est vide - &lt;a href=&quot;javascript:history.back();&quot;&gt
;Retour au formulaire&lt;/a&gt;&lt;/h3&gt;';exit;
}

if(isset($_POST['objet']
) &amp;&amp; !empty($_POST['objet'])){
$objet=$_POST['objet'];$error=NULL;
//f
iltrage
$objet=PetitClean($objet,100);
}else{
echo $error='&lt;h3 align=&quot
;center&quot;&gt;L\'objet est vide - &lt;a href=&quot;javascript:history.back();
&quot;&gt;Retour au formulaire&lt;/a&gt;&lt;/h3&gt;';exit;
}

if(isset($_POST
['message']) &amp;&amp; !empty($_POST['message'])){
$message=$_POST['message'];
$error=NULL;
//filtrage
$message=PetitClean($message,300);
}else{
echo $erro
r='&lt;h3 align=&quot;center&quot;&gt;Le message est vide - &lt;a href=&quot;jav
ascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&gt;&lt;/h3&gt;';exit
;
}

if(VerifierAdresseMail($mail)){
//echo 'mail ok';
}else{
echo $error=
'&lt;h3 align=&quot;center&quot;&gt;Votre adresse e-mail n\'est pas valide - &lt
;a href=&quot;javascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&gt;
&lt;/h3&gt;';exit;
}

if($_SERVER['REQUEST_METHOD']==='POST' &amp;&amp; isset
($_POST['code']) &amp;&amp; !empty($_POST['code']) &amp;&amp; $_POST['code']===$
_SESSION['verif']){ 

/*un mail, un enregistrement mysql, une ouverture de fic
hier ... un traitement */

$destinataire=&quot;xxxxxxxxxxxxxxx@free.fr&quot;; 
 /*ICI LE MAIL QUI RECEPTIONNE*/
$subject=$objet;
$body=$message;

/*format 
du mail*/
$headers = &quot;MIME-Version: 1.0\r\n&quot;;
$headers .= &quot;Cont
ent-type: text/plain; charset=iso-8859-1\r\n&quot;;
/*ici on détermine l'expedi
teur et l'adresse de réponse*/
$headers .= &quot;From: $nom &lt;$mail&gt;\r\nRe
ply-to : $nom &lt;$mail&gt;\nX-Mailer:PHP&quot;;
/*tout est ok*/
    
    if 
(mail($destinataire,$subject,$body,$headers)){
    /*petite secu*/
    $messag
e=NULL;
    $mail=NULL;
    $nom=NULL;
    $objet=NULL;
    $_POST=NULL;
  
  $_SESSION['verif']=NULL; /*anti double post*/
    $destinataire=NULL;
    ec
ho '&lt;h3 align=&quot;center&quot;&gt;Votre message est envoyé, merci ! - &lt;a
 href=&quot;javascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&gt;&l
t;br /&gt;&lt;/h3&gt;';exit; 
    /* ou redirection header('Location: <a href='
http://unsite.fr/merci.html');exit;' target='_blank'>http://unsite.fr/merci.html
');exit;</a> ... */
    }else{
    /*petite secu*/
    $message=NULL;
    $m
ail=NULL;
    $nom=NULL;
    $objet=NULL;
    $_POST=NULL;
    $_SESSION['ve
rif']=NULL;  /*anti double post*/
    $destinataire=NULL;
    echo '&lt;h3 ali
gn=&quot;center&quot;&gt;Désolé votre message n\'a pas pu être envoyé ! - &lt;a 
href=&quot;javascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&gt;&lt
;br /&gt;&lt;/h3&gt;';exit;
    /* ou redirection header('Location: <a href='ht
tp://unsite.fr/erreur.html');exit;' target='_blank'>http://unsite.fr/erreur.html
');exit;</a> ... */
    }

/*petite secu*/
$message=NULL;
$mail=NULL;
$nom
=NULL;
$objet=NULL;
$_POST=NULL;
$destinataire=NULL;

} else {
echo $error
='&lt;h3 align=&quot;center&quot;&gt;ERREUR SUR LE CODE DE SECURITE - &lt;a href
=&quot;javascript:history.back();&quot;&gt;Retour au formulaire&lt;/a&gt;&lt;/h3
&gt;';exit;
}
?&gt;
</pre>
<br /><a name='conclusion'></a><h2> Conclusion : 
</h2>
<br />- Ce petit bout de code est facilement adaptable &agrave; divers s
ituation, 4 fichiers.
<br />- Dans le zip c'est un 'form to mail' mais cela peu
t devenir un formulaire de login ou d'annonces ...
<br />- Le reste est dans le
 zip.
<br />- La pr&eacute;sentation est spartiate (je sais, je suis pas un des
igner)
<br />- Si vous utilisez mon code un petit mail pour le fun avec votre p
age.
<br />
<br />bne prog
<br />
<br />a++
