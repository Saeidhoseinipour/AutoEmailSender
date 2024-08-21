# AutoEmailSender

```python
import smtplib
import ssl
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders
import os

class EmailSender:
    def __init__(self, sender_email, password, pdf_filename):
        """
        Initializes the EmailSender class with the sender's email, password, and the PDF file to attach.
        
        :param sender_email: The email address of the sender.
        :param password: The password for the sender's email account.
        :param pdf_filename: The path to the PDF file that will be attached to the emails.
        """
        self.sender_email = sender_email
        self.password = password
        self.pdf_filename = pdf_filename
        self.context = ssl.create_default_context()

    def send_emails(self, recipients, subject, body_template):
        """
        Sends an email with a PDF attachment to a list of recipients.
        
        :param recipients: A list of tuples, each containing a recipient's name and email address.
        :param subject: The subject of the email.
        :param body_template: The template for the body of the email, with a placeholder for the recipient's name.
        """
        with smtplib.SMTP_SSL("smtp.gmail.com", 465, context=self.context) as server:
            server.login(self.sender_email, self.password)

            for name, recipient_email in recipients:
                msg = MIMEMultipart()
                msg['From'] = self.sender_email
                msg['To'] = recipient_email
                msg['Subject'] = subject

                # Personalize the email body
                body_personalized = body_template.format(name=name)
                msg.attach(MIMEText(body_personalized, "plain"))

                # Attach the PDF file
                self.attach_pdf(msg)

                # Send the email
                server.sendmail(self.sender_email, recipient_email, msg.as_string())

        print("Emails sent successfully.")

    def attach_pdf(self, msg):
        """
        Attaches a PDF file to the email message.
        
        :param msg: The MIMEMultipart email message object.
        """
        with open(self.pdf_filename, "rb") as attachment:
            part = MIMEBase("application", "octet-stream")
            part.set_payload(attachment.read())

        encoders.encode_base64(part)
        part.add_header(
            "Content-Disposition",
            f"attachment; filename= {os.path.basename(self.pdf_filename)}",
        )

        msg.attach(part)

# Example usage
if __name__ == "__main__":
    sender_email = 'saeidhoseinipour9@gmail.com'
    password = 'ibbjzrknliplumqz'
    #sender_email = "your_email@gmail.com"
    #password = os.getenv('EMAIL_PASSWORD')  # Use environment variable for security
    pdf_filename = "D:\\My papers\\Apply\\CV_customize\\CV_Version5.pdf"

    # List of recipients (name, email)
    recipients = [
        ("Saeid", "saeidhoseinipour7@gmail.com"),
        ("AnitA Vafaei", "anitavafaee@gmail.com")
        #("Alice Johnson", "alice.johnson@example.com"),
        # Add more recipients as needed
    ]

    # Subject and body of the email
    subject = "Special PDF Attachment"
    body_template = """
    Dear {name},

در اعماق اقیانوس بیکران، جایی که امواج در هم می‌پیچند و طوفان‌ها بی‌وقفه می‌غرند، کشتی‌ای کوچک و تنها در دل شب‌های تاریک دریا به سفر مشغول بود. ناخدای این کشتی، مردی بود که سال‌ها تنها زندگی کرده و شب‌ها در سکوت و تنهایی به نوشتن مقالات علمی می‌پرداخت. او آدمی بود که تمرکز و سکوت را مقدس می‌دانست، چرا که این دو عنصر به او قدرت می‌دادند تا ذهن منطقی و پر استدلال خود را به کار گیرد.

ناخدا که به تنهایی عادت کرده بود، هیچ‌گاه نیازی به حضور کسی دیگر در کشتی‌اش نمی‌دید. اما روزی سرنوشت، زنی به نام حوا را وارد زندگی‌اش کرد. حوا با طبعی برونگرا و بی‌قرار، درست نقطه مقابل ناخدا بود. او سرشار از شور و هیجان بود و به دنبال کشتی بزرگ‌تری می‌گشت، جایی که بتواند همه دنیا را تسخیر کند.

روز اولی که حوا پا به کشتی گذاشت، ناخدا در عمق شب و سکوت غرق بود و حوا که از هیاهوی دنیا بیرون آمده بود، نمی‌توانست سکوت ناخدا را تحمل کند. او از ناخدا و کشتی قبلی‌اش می‌پرسید و با بی‌صبری منتظر بود تاة داستان‌های ناخدا را بشنود. اما ناخدا که به تمرکز و آرامش خود نیاز داشت، از این همه شلوغی و بی‌قراری حوا خسته شده بود.

کم‌کم، تضاد میان این دو آشکارتر شد. حوا که هنوز بادبان کشتی نشده بود، سعی می‌کرد به روش خودش فرماندهی را به دست گیرد. او نقشه‌های ناخدا را زیر سوال می‌برد و گاهی حتی به قدرت ناخدا شک می‌کرد. در همین حین، ناخدا احساس می‌کرد که کنترل کشتی از دستش خارج شده و دیگر نمی‌تواند تصمیم‌گیرنده نهایی باشد.

روزها و شب‌ها گذشت، و کشتی همچنان در دریای ناآرام به پیش می‌رفت. حوا که حالا بادبان کشتی شده بود، گاه به جنوب و گاه به شمال می‌کشید، در حالی که ناخدا هنوز تلاش می‌کرد کشتی را به سمت مقصدی مشخص هدایت کند. تضاد بین آنها چنان بود که گویی هرکدام در جهتی مخالف دیگری حرکت می‌کرد.

نتیجه این تضاد، سکون و ایستایی بود. کشتی که می‌بایست در دریای بی‌پایان به سوی مقصدی پیش رود، حالا در جایی متوقف شده بود. سکون، آفتی بود که به جان و قلب ناخدا رخنه کرده و او را به فکر فرو برده بود.

اما ناخدا می‌دانست که سکون، دشمن جان و روح است و حرکت تنها پادزهر آن. او باید راهی پیدا می‌کرد تا حوا را با خود همسو کند، تا بتوانند با هم کشتی را به سوی مقصدی بزرگ‌تر و والاتر هدایت کنند.

آیا ناخدا و حوا می‌توانستند این تضاد را کنار بگذارند و با هم به سوی آینده‌ای روشن حرکت کنند؟ یا اینکه کشتی‌شان برای همیشه در دل دریاهای طوفانی، در سکون و سکوت باقی می‌ماند؟ این سوالی بود که تنها زمان می‌توانست پاسخ دهد.
    Best regards,
    {name}
    """

    # Create an EmailSender instance
    email_sender = EmailSender(sender_email, password, pdf_filename)

    # Send emails
    email_sender.send_emails(recipients, subject, body_template)

```
