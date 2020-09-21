# Sending email by SendGrid in Symfony 4, 5
This repository shows how to send emails by using the Mailer Component and SendGrid API Key in Symfony 4 and 5


# Installation
We need to install Mailer and SendGrid support with composer command. If you do not have composer, you should install it first.

`composer require symfony/mailer`

`composer require symfony/sendgrid-mailer`

These installations will make some changes in your Symfony configuration.

The important one is in .env file. You will see something like below added to your env file

```
###> symfony/sendgrid-mailer ###

#SENDGRID_KEY=

#MAILER_DSN=smtp://$SENDGRID_KEY@sendgrid

###< symfony/sendgrid-mailer ###
```

We need to uncomment second and third lines and put our SendGrid API Key. SendGrid API Key should be in the format of API_ID.API_Key something like `SG.xxxxx.xxxxxx`

It should look like below at the end.

```
###> symfony/sendgrid-mailer ###

SENDGRID_KEY=SG.xxxxx.xxxxxx

MAILER_DSN=smtp://$SENDGRID_KEY@sendgrid

###< symfony/sendgrid-mailer ###
```


Once we are done here in the configuration we can use Mailer to send email.

# Regular Email

```php
// src/Controller/MailerController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Mailer\MailerInterface;
use Symfony\Component\Mime\Email;

class MailerController extends AbstractController
{
  public function sendEmail(MailerInterface $mailer)
  {
    //create and send email with twig template
    $email = (new Email())
          ->from("sender@example.com")
          ->to("email@example.com")
          ->subject("Welcome!")
          ->text("I am sending my first email!")
          ->html("<p>Hey! This is my first email</p>");

    $mailer->send($email);
  }    
}
```

# Twig Templated Email

```php
// src/Controller/MailerController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Mailer\MailerInterface;
use Symfony\Component\Mime\Email;

class MailerController extends AbstractController
{
  public function sendEmail(MailerInterface $mailer)
  {
    //create and send email with twig template
    $email = (new TemplatedEmail())
          ->from(new NamedAddress("sender@example.com", "Me"))
          ->to(new NamedAddress("email@example.com", "John"))
          ->subject("Welcome!")
          ->context([
              'datetime' => \DateTime("m/d/Y")
          ])
          ->htmlTemplate('email/welcome.html.twig');

    $mailer->send($email);
  }    
}
```

For more options and detaild information you can visit Symfony Official documentation for [Mailer](https://symfony.com/doc/current/mailer.html).
