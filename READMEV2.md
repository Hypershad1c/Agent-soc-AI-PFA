flowchart TD

%% ============================
%%        SOURCES DE LOGS
%% ============================

A[Sources de Logs<br>Firewall / IDS / Windows Logs / DNS / Proxy / Netflow] --> B

%% ============================
%%  INGESTION & PIPELINES ELK
%% ============================

B[Ingestion Logs (Beats + Logstash)<br>- Filebeat<br>- Winlogbeat<br>- Packetbeat<br>- Logstash Parsing] --> C[Elasticsearch<br>Stockage + Recherche]

%% ============================
%%  API IA (FASTAPI)
%% ============================

C --> D[API IA (FastAPI)<br>• Charge model.pkl<br>• Extraction Features<br>• Prédiction (Normal / Suspicious / Attack)]

%% ============================
%%      FRONTEND WEB SOC
%% ============================

D --> E[Frontend Web<br>Dashboard Temps Réel<br>Graphiques / Scores IA]

%% ============================
%%    ANALYSTE SOC HUMAIN
%% ============================

E --> F[Analyste SOC<br>Analyse des Alertes<br>Décision: Auto ou Manuelle]

%% ============================
%%        PLAYBOOKS
%% ============================

F --> G[Playbooks Automatisés<br>- block_ip.yaml<br>- alert_mail.yaml<br>- isolate_host.yaml]

%% ============================
%%  ACTIONS AUTOMATIQUES
%% ============================

G --> H[ACTIONS SOC<br>• Blocage IP<br>• Quarantaine Machine<br>• Désactivation Compte<br>• Notification]

%% ============================
%%   PHASE IA – MACHINE LEARNING
%% ============================

subgraph IA_Training [Phase IA – Entraînement du Modèle (Google Colab)]
    I1[Datasets<br>CTU-13 / CICIDS2017 / UNSW-NB15 / Bot-IoT] --> I2[Notebook Colab<br>Feature Engineering + Training]
    I2 --> I3[Export model.pkl]
end

I3 --> D

%% ============================
%%          INFRA DOCKER
%% ============================

subgraph Docker_Infra [Infrastructure Docker]
    J1[Elasticsearch]
    J2[Kibana]
    J3[FastAPI - API IA]
    J4[Frontend Web Dashboard]
end
