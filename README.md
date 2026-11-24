# Mapa Fácil com GPS — IFTO Campus Palmas

Aplicação web responsiva e instalável (**PWA / App Mobile**) para localização interna e navegação GPS pedestre no **Instituto Federal do Tocantins (IFTO) — Campus Palmas**.

Permite pesquisar blocos, salas de aula, laboratórios, coordenações e áreas comuns, exibindo o destino no mapa de satélite de alta resolução e traçando a rota a pé pelas passarelas e corredores reais do campus.

---

## Estrutura do Projeto

```text
mapa-ifto-palmas-app/
├── index.html                          # Estrutura principal da interface
├── manifest.json                       # Configuração PWA (ícone, nome, modo standalone)
├── sw.js                               # Service Worker para cache e funcionamento offline
├── capacitor.config.json               # Configuração pronta para gerar APK/AAB (Android) ou iOS
├── package.json                        # Scripts auxiliares para servidor local e Capacitor
├── .gitignore                          # Arquivos ignorados pelo Git
├── README.md                           # Documentação do projeto
├── css/
│   └── style.css                       # Estilos visuais e layout responsivo para celular
├── js/
│   └── app.js                          # Mapa Leaflet, algoritmo de rotas (Dijkstra) e GPS
├── data/
│   ├── campus-data.js                  # Base geoespacial carregada diretamente pela aplicação
│   └── campus-ifto-palmas.geojson.json # Exportação GeoJSON padrão para uso em SIG/QGIS
└── icons/
    └── icon.svg                        # Ícone vetorial do aplicativo
```

---

## Funcionalidades Principais

* **Base Geoespacial Completa (522 elementos):**
  * **38 Edificações:** Polígonos georreferenciados dos blocos administrativos, acadêmicos, biblioteca, refeitório e áreas esportivas.
  * **445 Pontos de Interesse:** Salas, laboratórios, coordenações, setores administrativos, banheiros, bebedouros e áreas de convivência.
  * **39 Caminhos e Passarelas:** Malha pedestre com metadados de cobertura e tipo de piso.
* **Motor de Roteamento Pedestre (Dijkstra):** Constrói um grafo topológico a partir das passarelas do campus, projeta origem e destino no segmento mais próximo e calcula distância (em metros) e tempo estimado de caminhada.
* **GPS em Tempo Real:** Integração com a API de Geolocalização do dispositivo (`navigator.geolocation`), atualizando o trajeto dinamicamente conforme o usuário caminha pelo campus (com fallback automático para a **Entrada Principal**).
* **Camadas Sob Demanda:** O mapa inicia limpo e exibe apenas o local pesquisado ou as camadas ativadas manualmente pelo usuário.

---

## Como Publicar no GitHub e Colocar no Ar (GitHub Pages)

1. Crie um novo repositório no GitHub (ex: `mapa-ifto-palmas`).
2. Extraia o conteúdo desta pasta e envie os arquivos para o repositório:
   ```bash
   git init
   git add .
   git commit -m "Versão inicial do Mapa Fácil com GPS - IFTO Campus Palmas"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/mapa-ifto-palmas.git
   git push -u origin main
   ```
3. Para hospedar gratuitamente com HTTPS (obrigatório para o GPS funcionar no celular):
   * No GitHub, acesse **Settings → Pages**.
   * Em **Source**, selecione **Deploy from a branch**, escolha `main` e `/ (root)`, e clique em **Save**.
   * Em poucos instantes seu link `https://seu-usuario.github.io/mapa-ifto-palmas/` estará ativo.

---

## Como Transformar em Aplicativo (Celular)

### Opção 1: Instalação Direta como PWA (Sem Loja de Aplicativos)
1. Abra o link do **GitHub Pages** no navegador do celular (Chrome no Android ou Safari no iOS).
2. Toque no menu do navegador e selecione **"Adicionar à tela inicial"** ou **"Instalar aplicativo"**.
3. O app será instalado com ícone próprio, tela cheia e cache offline.

### Opção 2: Gerar App Nativo Android (APK / Play Store) com Capacitor
Este projeto já inclui o `capacitor.config.json` configurado. Com o Node.js e o Android Studio instalados:
```bash
npm run android:init
npm run android:sync
npm run android:open
```
No Android Studio, lembre-se de manter a permissão de localização no `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
