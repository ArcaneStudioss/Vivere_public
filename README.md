# Vivere Loja

E-commerce próprio de uma empresa de marmitas congeladas, em produção. Substituiu uma plataforma de cardápio de terceiros e conversa com um bot de WhatsApp (Vivi) que usa o mesmo banco.

> Estudo de caso. O código-fonte é privado porque o projeto roda em um negócio real.

## Problema
A empresa vendia por plataforma de terceiros, sem controle de estoque, de entrega por cidade nem de nota fiscal, e com atendimento manual no WhatsApp.

## O que foi construído
- **Vitrine e checkout** sem login: filtros, combos "monte o seu", frete grátis progressivo, Pix próprio com QR Code e cartão em até 12x.
- **Entrega por cidade e dia**, com regra de corte de horário e reserva de estoque ao criar o pedido.
- **Painel administrativo:** pedidos, lista de produção do dia, estoque com movimentações, cupons, relatórios com exportação em CSV.
- **Fiscal:** emissão automática de NF-e (B2B) e NFC-e (consumidor final) via Focus NFe.
- **Vivi, bot de WhatsApp:** atende por texto e áudio (Whisper hospedado), anota pedidos e devolve um link com o carrinho montado.

## Decisões técnicas que valem destaque
- **Preço nunca vem do navegador.** O servidor recalcula tudo; um filtro de segurança também barra preço inventado pelo bot.
- **Pagamento só aprovado depois de reconferido** na API do gateway (o webhook não é assinado).
- **Deploy sem queda:** duas réplicas atrás de proxy com healthcheck, reiniciadas uma de cada vez, com rollback.
- **Migrações automáticas e aditivas** na subida, com teste que barra `drop`. Se a migração falha, a versão anterior continua no ar.
- Limite de conexões do pooler respeitado por réplica, depois de um incidente de esgotamento de conexões.

## Stack
Next.js · TypeScript · PostgreSQL (Supabase) · Docker · Traefik · n8n · Evolution API · Focus NFe · Whisper

## Autor
Gabriel · [GitHub](https://github.com/ArcaneStudioss)
