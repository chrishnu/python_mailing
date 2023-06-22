
#Installation de la librairie : pip install secure-smtplib

import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders

# Informations d'authentification pour le compte Gmail
smtp_server = 'smtp.gmail.com'
smtp_port = 587
smtp_username = '***********************'
smtp_password = '*******************'(mot de passe d'application à généré dans Gmail, c est sur 16 positions)

sender_email = '**********************'
receiver_email = '*********************j'

# Création de l'objet du message
message = MIMEMultipart()
message['From'] = sender_email
message['To'] = receiver_email
message['Subject'] = 'TEST D\'ENVOI DE MAILS AVEC PYTHON'

# Corps du message
message.attach(MIMEText('Bonjour Monsieur. Veuillez recevoir ci-joint mon cahier de charges.', 'plain'))

# Chemin de la pièce jointe
file_path = r'C:\Users\hp\Documents\cahier_de_charges.pdf'

# Lecture du fichier
with open(file_path, 'rb') as attachment:
    # Création de l'objet de pièce jointe
    part = MIMEBase('application', 'octet-stream')
    part.set_payload(attachment.read())

# Encodage de la pièce jointe en Base64
encoders.encode_base64(part)

# Ajout des en-têtes de la pièce jointe
part.add_header('Content-Disposition', f'attachment; filename=attachment.pdf')

# Ajout de la pièce jointe au message
message.attach(part)

# Gestion des erreurs avant l'envoi
try:
    server = smtplib.SMTP(smtp_server, smtp_port)
    server.starttls()
    server.login(smtp_username, smtp_password)
    server.sendmail(sender_email, receiver_email, message.as_string())
    print('E-mail envoyé avec succès')
except Exception as e:
    print('Une erreur s\'est produite lors de l\'envoi de l\'e-mail:', str(e))
finally:
    server.quit()
