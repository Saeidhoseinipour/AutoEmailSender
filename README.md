# AutoEmailSender


![Alt text](URL_or_path_to_image)


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
    sender_email = 'your_email@gmail.com'
    password = 'your_passs'
    pdf_filename = "youe_path_attachment_file"

    # List of recipients (name, email)
    recipients = [
        ("Alice Johnson", "alice.johnson@example.com")
        # Add more recipients as needed
    ]

    # Subject and body of the email
    subject = "Special PDF Attachment"
    body_template = """
    Dear {name},

    Best regards,
    {name}
    """

    # Create an EmailSender instance
    email_sender = EmailSender(sender_email, password, pdf_filename)

    # Send emails
    email_sender.send_emails(recipients, subject, body_template)

```
