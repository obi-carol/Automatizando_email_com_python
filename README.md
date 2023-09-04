# Automatizando_email_com_python
Segue uam automatização simples de e-mails.
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.application import MIMEApplication

# Configurações do servidor SMTP
smtp_server = "smtp.seudomínio.com"  # Substitua pelo servidor SMTP da sua organização
smtp_port = 587  # Substitua pela porta do servidor SMTP da sua organização
smtp_username = "seu_email@seudomínio.com"  # Substitua pelo seu endereço de e-mail
smtp_password = "sua_senha"  # Substitua pela sua senha

# Lista de colaboradores (endereços de e-mail)
colaboradores = ["colaborador1@seudomínio.com", "colaborador2@seudomínio.com"]

# Mensagem de e-mail
assunto = "Relatório de Monitoria"
corpo = """
Olá,

Segue em anexo o relatório da última monitoria. Destacamos alguns pontos importantes:

1. Ponto 1
2. Ponto 2
3. Ponto 3

Atenciosamente,
Sua Empresa
"""

# Anexos (lista de caminhos dos arquivos a serem anexados)
anexos = ["caminho_do_arquivo1.pdf", "caminho_do_arquivo2.pdf"]

# Configurar e enviar e-mails
try:
    server = smtplib.SMTP(smtp_server, smtp_port)
    server.starttls()
    server.login(smtp_username, smtp_password)

    for colaborador in colaboradores:
        # Criar o objeto MIMEMultipart
        msg = MIMEMultipart()
        msg["From"] = smtp_username
        msg["To"] = colaborador
        msg["Subject"] = assunto

        # Adicionar o corpo do e-mail
        msg.attach(MIMEText(corpo, "plain"))

        # Adicionar anexos
        for arquivo in anexos:
            with open(arquivo, "rb") as file:
                part = MIMEApplication(file.read(), Name=arquivo)
            part["Content-Disposition"] = f'attachment; filename="{arquivo}"'
            msg.attach(part)

        # Enviar o e-mail
        server.sendmail(smtp_username, colaborador, msg.as_string())

    print("E-mails enviados com sucesso!")

except Exception as e:
    print("Erro ao enviar e-mails:", str(e))

finally:
    server.quit()
