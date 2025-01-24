# Site Bankizi

WARNING: **Status da aplicação**
**Em produção**


É o site oficial da [Bankizi](https://www.bankizi.com/).


<h4>Tecnologias utilizadas</h4>

- React
- Tailwind
- MaterialUI (MUI)
- Next

<h4>Funcionamento</h4>

Por se tratar de uma Landing Page (LP) estática a aplicação não possui nenhuma regra complexa. Contudo possui dois pontos de atenção:

- Possui um formulário de contato que dispara um e-mail (utilizando o serviço Simple Email Service - SES da AWS e nodeMailer) para o e-mail de suporte da bankizi, informando que houve uma tentativa de contato através do formulário.
- Possui um script do Google Tag Manager para monitoração de acessos ao site.

