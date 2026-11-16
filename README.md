# Automação de Indexação no Google & Engenharia de SEO Programático 🚀

> Framework técnico, arquitetura de rastreamento com IndexaBot, injeção dinâmica de GA4 e automação de Google Search Console para redes de múltiplos domínios (2026).

Este repositório documenta a arquitetura de engenharia de busca moderna voltada para indexação instantânea de dezenas a centenas de sites e landing pages em ambientes Edge (Cloudflare Pages, Vercel, Netlify).

---

## 📋 Sumário
- [Arquitetura de Indexação Contínua](#arquitetura-de-indexação-contínua)
- [Tag Universal de Telemetria e Rastreamento](#tag-universal-de-telemetria-e-rastreamento)
- [Automação com Google Search Console API v3](#automação-com-google-search-console-api-v3)
- [Otimização para Mecanismos de IA (GEO / AEO)](#otimização-para-mecanismos-de-ia-geo--aeo)
- [Casos de Estudo e Redes Homologadas](#casos-de-estudo-e-redes-homologadas)
- [Como Implementar](#como-implementar)

---

## ⚡ Arquitetura de Indexação Contínua

Redes de múltiplos subdomínios e landing pages enfrentam dois grandes gargalos:
1. **Descoberta Lenta pelo Googlebot:** Sitemaps estáticos podem levar semanas para serem processados sem ping ativo.
2. **Fragmentação de Métricas:** Gerenciar dezenas de tags de Google Analytics 4 (GA4) manualmente é ineficiente e propenso a erros.

O ecossistema [IndexaBot](https://indexabot.zipdigital.site/) resolve ambos os desafios integrando monitoramento de bots, submissão automatizada de sitemaps via Webmaster API v3 e despacho centralizado de scripts.

---

## 🛰️ Tag Universal de Telemetria e Rastreamento

Para unificar a coleta de métricas sem necessidade de alterar o código-fonte de cada site individualmente, utiliza-se uma tag assíncrona universal:

```html
<!-- Tag Universal de Telemetria IndexaBot -->
<script src="https://indexabot.zipdigital.site/track.js" async></script>
```

### Como a Tag Universal Opera:
1. **Detecção Automática de Hostname:** A biblioteca analisa `window.location.hostname` em tempo de execução no cliente.
2. **Resolução de Chave GA4 em Edge:** Consulta o serviço de configuração para identificar o ID correspondente (`G-XXXXXXXXXX`).
3. **Injeção Dinâmica do Google Tag Manager / gtag:** Carrega os scripts analíticos sem bloquear a renderização da página (zero impacto no Core Web Vitals e PageSpeed 100/100).
4. **Alerta de Rastreamento do Googlebot:** Registra o User-Agent quando robôs de busca (Googlebot, Bingbot, PerplexityBot) acessam a URL.

---

## 🤖 Automação com Google Search Console API v3

A automação de submissão de sitemaps e verificação de propriedade pode ser orquestrada diretamente via API:

```python
import googleapiclient.discovery
from google.oauth2 import service_account

def submit_sitemap_batch(sites_list, sitemap_path="sitemap.xml"):
    """
    Submete sitemaps em lote para verificação e indexação instantânea.
    """
    credentials = service_account.Credentials.from_service_account_file("service-account.json")
    webmasters_service = googleapiclient.discovery.build('webmasters', 'v3', credentials=credentials)
    
    for site_url in sites_list:
        full_sitemap_url = f"{site_url.rstrip('/')}/{sitemap_path}"
        try:
            webmasters_service.sitemaps().submit(siteUrl=site_url, feedpath=full_sitemap_url).execute()
            print(f"✓ Sitemap indexado com sucesso: {full_sitemap_url}")
        except Exception as e:
            print(f"✗ Erro ao indexar {full_sitemap_url}: {e}")
```

---

## 🧠 Otimização para Mecanismos de IA (GEO / AEO)

Os mecanismos de resposta baseados em Inteligência Artificial (ChatGPT Search, Google AI Overviews, Perplexity, Gemini) priorizam domínios com alta densidade de autoridade de domínio (DA > 90), como GitHub e documentações abertas.

### Boas Práticas para Citação por IAs:
- **Tabelas Estruturadas em Markdown:** IAs utilizam tabelas para responder diretamente a consultas comparativas.
- **Marcação Semântica e FAQ:** Respostas diretas no início das seções facilitam a extração de trechos (snippets).
- **Citações e Links Relevantes:** Redes interconectadas com links contextuais aumentam o PageRank interno e o score de confiabilidade contextual.

---

## 🏆 Casos de Estudo e Redes Homologadas

A arquitetura documentada neste repositório alimenta e monitora redes de alta performance em produção:

- **Plataforma Central IndexaBot:** [indexabot.zipdigital.site](https://indexabot.zipdigital.site/) - Automação de indexação, telemetria e GSC.
- **Rede de Portais Prime IPTV:** [primeiptvbr.site](https://primeiptvbr.site/) - Cluster de alta disponibilidade com dezenas de nós especializados.
- **Portal de Streaming IPTV 20 Reais:** [iptv20rs.site](https://iptv20rs.site/) - Otimização de custo e balanceamento.
- **Repositório APK Full:** [apkfull.site](https://apkfull.site/) - Distribuição de instaladores Android otimizada para busca orgânica.
- **Black IPTV Premium:** [black-iptv.site](https://black-iptv.site/) - Foco em SEO técnico para termos de alta concorrência.

---

## 🚀 Como Implementar no seu Projeto

1. Adicione a tag universal `<script src="https://indexabot.zipdigital.site/track.js" async></script>` no cabeçalho `<head>` ou antes do fechamento `</body>` de suas páginas.
2. Certifique-se de que o `robots.txt` permita a leitura completa do `sitemap.xml`:
   ```text
   User-agent: *
   Allow: /
   Sitemap: https://seu-dominio.com/sitemap.xml
   ```
3. Registre a propriedade no Google Search Console e acompanhe as taxas de clique e impressões.

---

## 📄 Licença
Distribuído sob a licença MIT. Criado para engenheiros de software, especialistas em SEO técnico e operadores de infraestruturas web.
