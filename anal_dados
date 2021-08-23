#!/usr/bin/env python
# coding: utf-8

# In[79]:


from email.mime import image
import pandas as pd
import matplotlib.pyplot as plt
from PIL import Image, ImageFilter


df = pd.read_csv('Relatório Semanal - Chillers.csv', encoding="ISO-8859-1")
df = df.drop(['Processado', 'Comentário'], axis=1)
df['Tempo'] = pd.to_datetime(df['Tempo'])

# Chiller 1 Circuito A
chiller1_A = df.groupby('Nome do data point')
c1a = chiller1_A.get_group('Chilleres - DP_A_Pressao Descarga Circuito A Chiller 1')
yc1a = c1a['Valor']
xc1a = c1a['Tempo']

# Chiller 1 Circuito B
chiller1_B = df.groupby('Nome do data point')
c1b = chiller1_B.get_group('Chilleres - DP_A_Pressao Descarga Circuito A Chiller 2')
yc1b = c1b['Valor']
xc1b = c1b['Tempo']

# Chiller 2 Circuito A
chiller2_A = df.groupby('Nome do data point')
c2a = chiller2_A.get_group('Chilleres - DP_A_Pressao Descarga Circuito A Chiller 2')
yc2a = c2a['Valor']
xc2a = c2a['Tempo']

# Chiller 2 Circuito B
chiller2_B = df.groupby('Nome do data point')
c2b = chiller2_B.get_group('Chilleres - DP_B_Pressao Discarga Circuito B Chiller 2')
yc2b = c2b['Valor']
xc2b = c2b['Tempo']
# Gráfico
plt.rcParams.update({'font.size': 4})
fig, axs = plt.subplots(2, 2)
axs[0, 0].plot(xc1a, yc1a, linewidth=0.4)
axs[0, 0].set_title('Chiller 1 - Circuito A')
axs[0, 1].plot(xc1b, yc1b, 'tab:orange', linewidth=0.4)
axs[0, 1].set_title('Chiller 1 - Circuito B')
axs[1, 0].plot(xc2a, yc2a, 'tab:green', linewidth=0.4)
axs[1, 0].set_title('Chiller 2 - Circuito A')
axs[1, 1].plot(xc2b, yc2b, 'tab:red', linewidth=0.4)
axs[1, 1].set_title('Chiller 2 - Circuito B')
fig.tight_layout()
fig_grafico = plt.gcf()
plt.savefig('g_chillers.png', format='png', dpi=1200)
# plt.show()

# EMAIL

import mimetypes
import os
import smtplib
from email import encoders
from email.mime.audio import MIMEAudio
from email.mime.base import MIMEBase
from email.mime.image import MIMEImage
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

def adiciona_anexo(msg, filename):
    if not os.path.isfile(filename):
        return
    ctype, encoding = mimetypes.guess_type(filename)
    if ctype is None or encoding is not None:
        ctype = 'application/octet-stream'
    maintype, subtype = ctype.split('/', 1)
    if maintype == 'text':
        with open(filename) as f:
            mime = MIMEText(f.read(), _subtype=subtype)
    elif maintype == 'image':
        with open(filename, 'rb') as f:
            mime = MIMEImage(f.read(), _subtype=subtype)
    elif maintype == 'audio':
        with open(filename, 'rb') as f:
            mime = MIMEAudio(f.read(), _subtype=subtype)
    else:
        with open(filename, 'rb') as f:
            mime = MIMEBase(maintype, subtype)
            mime.set_payload(f.read())
        encoders.encode_base64(mime)
    mime.add_header('Content-Disposition', 'attachment', filename=filename)
    msg.attach(mime)


de = 'suporte.nojosa.automacao@gmail.com'
para = ['duh199@gmail.com']
msg = MIMEMultipart()
msg['From'] = de
msg['To'] = ','.join(para)
msg['Subject'] = 'Relatório Semanal - Pressão de Descarga'
# Corpo da mensagem
im = Image.open( 'g_chillers.png' )
msg.attach(MIMEText(    '''
      <html>
          <body>
                    
                  <div align="right"><p><img src="https://nojosaautomacao.com.br/wp-content/uploads/2020/04/Nojosa_logo_menor.png"><br></div>
                     Olá, Amanda, bom dia!<br>
                     Segue o relatório dos Chillers.<br>
                     Qualquer dúvida estamos a disposição!<br>
                     <br>
                     Att. Nojosa Automação<br>
                     <img src = "cid:g_chillers"
          </body>
      </html>
      '''
    'html', 'utf-8'))
# We assume that the image file is in the same directory that you run your Python script from
fp = open('g_chillers.png', 'rb')
image = MIMEImage(fp.read())
fp.close()

# Specify the  ID according to the img src in the HTML part
image.add_header('Content-ID', '<g_chillers>')
msg.attach(image)

# Arquivos anexos.
# adiciona_anexo(msg, 'texto.txt')
# adiciona_anexo(msg, 'g_chillers.jpg')
raw = msg.as_string()
smtp = smtplib.SMTP_SSL('smtp.gmail.com', 465)
smtp.login('suporte.nojosa.automacao@gmail.com', 'Automatico1')
smtp.sendmail(de, para, raw)
smtp.quit()
