
# Implementando Monitoramento no Azure

### Como implementarar o monitoramento de máquinas virtuais no Azure

Para configurar e implementar o monitoramento de máquinas virtuais (VMs) no Azure, utilize o Azure Monitor, que oferece ferramentas para coletar e analisar dados de desempenho e logs, além de permitir a criação de alertas. Você pode começar seguindo um tutorial do Azure para coletar logs e métricas das suas VMs, e então configurar regras de coleta de dados (DCRs) para especificar quais informações coletar e como enviá-las para o Azure Monitor.

#### 1. Planeje a arquitetura de monitoramento
O Azure Monitor para VMs (VM insights) entrega telemetria em quatro camadas: host da VM, sistema operacional convidado, workloads e aplicativo (via Application Insights). Cada camada gera diferentes tipos de métricas e logs, e exige configurações específicas de agente e regras de coleta de dados.

#### 2. Crie um Log Analytics Workspace
No portal Azure, pesquise por “Log Analytics workspaces” e clique em “+ Criar”.
* Defina assinatura, grupo de recursos, nome e região
* Escolha plano de retenção de dados conforme SLA e orçamento
Esse workspace será o repositório central de todos os logs e métricas das VMs.

#### 3. Habilite o VM Insights
Em Azure Monitor > Insights > Máquinas Virtuais, selecione suas VMs e clique em “Habilitar".
* Associe-as ao Log Analytics Workspace criado
* O serviço implanta automaticamente a extensão Azure Monitor Agent nas VMs (para Windows e Linux) e configura o Data Collection Rule padrão.

#### 4. Ajuste Data Collection Rules (DCR)
Se precisar de contadores de desempenho ou logs adicionais (ex.: Event Logs específicos no Windows, syslog no Linux), vá em Azure Monitor > Data Collection Rules > + Criar.
* Selecione o workspace alvo e as fontes (Performance Counters, Windows Event, Syslog)
* Aplique a DCR a um ou mais recursos (VMs ou conjuntos de escala)
Isso garante a coleta só do que você precisa, otimizando custos e ingestão de dados.

#### 5. Configure Diagnostic Settings na VM
Ainda no recurso VM, abra “Diagnóstico” > “Configurações de diagnóstico” > + Adição.
* Marque métricas e logs de atividades (por exemplo, “Delete Virtual Machine” para rastrear exclusão de VMs)
* Envie para o mesmo Log Analytics Workspace (ou para Storage/Events Hub, se desejar)
Esses logs ficam disponíveis para consultas no Log Analytics e gatilhos de alerta.

#### 6. Defina alertas e Action Groups
a. Crie Action Groups (Azure Monitor > Alerts > Manage action groups) para agrupar canais de notificação (e-mail, SMS, webhook, runbook).

b. Em Alerts > + New alert rule:
* Scope = VM ou subscription
* Condition = métrica (CPU > 80%), ou atividade (Delete Virtual Machine)
* Action = selecione Action Group criado
* Nome e severidade da regra
Isso garante notificação pró-ativa de incidentes críticos.

#### 7. Visualize e analise dados
* Em Azure Monitor > Insights > VMs: painéis pré-configurados de CPU, disco, memória, mapa de dependências
* Em Log Analytics: use Kusto Query Language (KQL) para criar consultas personalizadas
* Monte dashboards no portal ou no Power BI para relatórios periódicos
Essa visibilidade facilita a identificação de tendências e gargalos de performance.

#### 8. Boas práticas e otimizações
* Agrupe VMs de um mesmo ambiente/demanda em workspaces separados para isolar custos
* Utilize nomes claros e padronizados para regras de alerta e Action Groups
* Avalie periodicamente o volume de dados ingeridos e ajuste contadores ou retenção
* Considere habilitar Azure Automation ou Runbooks para respostas automatizadas a eventos (por exemplo, reiniciar uma VM que falhou no boot).

#### Próximos passos recomendados:
* Explore a integração de métricas customizadas via Azure Monitor REST API
* Avalie o uso de dashboards avançados no Grafana
* Aprofunde-se em Application Insights para monitorar aplicações hospedadas nas VMs
* Considere políticas do Azure Policy para padronizar a implantação de agentes e diagnósticos em novas VMs.
