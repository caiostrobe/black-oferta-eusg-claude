# Black Friday 2026 · Vitalício · Escola de Ultrassom

Página de oferta do Vitalício (Black Friday 2026), versão v5. Arquivo único e autocontido (`index.html`): HTML, CSS, JS e imagens (webp em base64) no mesmo arquivo. Basta publicar em qualquer hospedagem estática.

## Como a página funciona

- **Botões da página** levam até a seção **"A oferta"** (`#oferta`).
- **Botões da seção "A oferta"** abrem o **popup de verificação por e-mail**.
- **WhatsApp flutuante** (canto inferior direito) abre `https://escoladeultrassom.com/comercial`.
- Contagem regressiva até **15/11/2026, 23h59 (Brasília)**, barra fixa no topo, calculadora de retorno, garantia de 15 dias.

## Popup, redirecionamento e webhooks

Configuração no bloco `CONFIG`, no último `<script>` do `index.html`:

| Chave | Uso |
|---|---|
| `WEBHOOK_VERIFICAR` | Webhook n8n que consulta o e-mail na planilha de alunos |
| `WEBHOOK_REGISTRO` | Webhook n8n que registra o contato + UTMs + link de destino (vazio = desligado) |
| `CHECKOUT.ALUNO_COM_DESCONTO` | Checkout quando o e-mail é encontrado |
| `CHECKOUT.ALUNO_VITALICIO_2025` | Checkout quando `PRODUTO` = Vitalício 2025 |
| `CHECKOUT.PADRAO` | Checkout de não-aluno (pede nome e telefone antes) |

Fluxo:
1. Pessoa informa o e-mail → `POST {"email": "..."}` para `WEBHOOK_VERIFICAR`.
2. Encontrado → mostra nome, telefone mascarado e produto; redireciona em 3 s.
3. Não encontrado → pede nome e telefone; redireciona para o checkout padrão.
4. Erro → "Tentar de novo" ou ir direto ao checkout.
5. O checkout recebe `name`, `nome`, `email`, `phone` (com 55), `telefone`, `produto` e repassa `utm_*`, `gclid` e `fbclid`.

A resposta do n8n pode ser JSON puro ou o HTML com `<iframe srcdoc="...">` (o parser trata os dois).

## Antes de publicar

- **CORS no n8n:** a consulta é feita pelo navegador. Os dois webhooks precisam permitir o domínio onde a página for publicada (Webhook node → *Allowed Origins (CORS)*).
- **LGPD:** hoje o webhook devolve nome, telefone e produto de qualquer e-mail consultado. Recomendado devolver só o tipo de checkout e o primeiro nome.
- **Pendentes:** `VIDEO_1_URL`, `VIDEO_2_URL`, `VIDEO_3_URL` e o nome final da Imersão de Carreira.
- Fontes vêm do Google Fonts.
