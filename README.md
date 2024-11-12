# Sistema de Gerenciamento de Contas de Energia
Documentação de Referência Técnica

## 1. Visão Geral

Este sistema foi projetado para gerenciar dados de faturas de energia elétrica de diferentes distribuidoras, permitindo o armazenamento estruturado, processamento e análise das informações.

### 1.1 Objetivos Principais
- Padronizar o armazenamento de dados de diferentes distribuidoras
- Garantir a integridade e rastreabilidade das informações
- Facilitar análises comparativas e relatórios gerenciais
- Gerenciar compensações de energia e saldos de geração
- Manter histórico completo de consumo e faturamento

## 2. Estrutura de Dados

### 2.1 Tabelas Principais

#### DISTRIBUIDORA
- Dados cadastrais das empresas distribuidoras
- Informações fiscais e de identificação
- Base para relacionamento com unidades consumidoras

#### UNIDADE_CONSUMIDORA
- Dados cadastrais dos pontos de consumo
- Classificação e características técnicas
- Informações de localização e identificação

#### FATURA
- Dados principais do faturamento
- Datas e períodos de referência
- Valores totais e informações fiscais

#### LEITURA
- Registros de medição
- Cálculos de consumo
- Demandas medidas e faturadas

### 2.2 Tabelas de Componentes Tarifários

#### CONSUMO_FATURADO
- Registros de consumo por tipo (TE/TUSD)
- Tarifas aplicadas
- Valores calculados

#### ENERGIA_COMPENSADA
- Registros de compensação de energia
- Diferentes tipos de compensação
- Valores e quantidades compensadas

#### BANDEIRA_TARIFARIA
- Registros de bandeiras aplicadas
- Períodos de vigência
- Valores adicionais cobrados

### 2.3 Tabelas de Tributos e Encargos

#### TRIBUTOS
- ICMS, PIS, COFINS
- Bases de cálculo
- Alíquotas e valores

#### ENCARGOS
- Contribuição de Iluminação Pública
- Outros encargos setoriais
- Valores e bases de cálculo

### 2.4 Tabelas de Histórico e Controle

#### HISTORICO_CONSUMO
- Registro mensal de consumo
- Demandas históricas
- Períodos de faturamento

#### SALDO_GERACAO
- Controle de geração distribuída
- Saldos de energia
- Prazos de expiração

## 3. Fluxo de Dados

### 3.1 Entrada de Dados
1. Cadastro de distribuidoras
2. Cadastro de unidades consumidoras
3. Importação de faturas

### 3.2 Processamento Principal
1. Registro de leituras
2. Cálculos de consumo
3. Aplicação de tarifas
4. Processamento de compensação
5. Aplicação de bandeiras

### 3.3 Cálculos Tributários
1. Determinação de bases de cálculo
2. Aplicação de alíquotas
3. Cálculo de encargos

### 3.4 Consolidação
1. Atualização de históricos
2. Gestão de saldos
3. Totalização de faturas
4. Validações finais

## 4. Regras de Negócio Importantes

### 4.1 Validações
- Consistência entre leituras e consumo
- Verificação de períodos de faturamento
- Conferência de valores calculados
- Validação de alíquotas por estado

### 4.2 Compensação de Energia
- Controle de saldos por unidade
- Prazos de expiração
- Prioridades de compensação
- Distribuição entre unidades

### 4.3 Tributação
- Regras específicas por estado
- Bases de cálculo diferenciadas
- Isenções e benefícios fiscais

## 5. Considerações de Implementação

### 5.1 Segurança
- Controle de acesso por perfil
- Criptografia de dados sensíveis
- Logs de alteração
- Backup e recuperação

### 5.2 Performance
- Indexação adequada
- Particionamento de tabelas
- Estratégias de arquivamento
- Otimização de consultas

### 5.3 Integridade
- Constraints de banco de dados
- Triggers de validação
- Procedimentos de consistência
- Rotinas de verificação

## 6. Exemplos de Uso

### 6.1 Consultas Comuns
```sql
-- Consumo por unidade
SELECT 
    uc.numero_cliente,
    SUM(l.consumo_kwh) as consumo_total,
    AVG(l.consumo_kwh) as consumo_medio
FROM UNIDADE_CONSUMIDORA uc
JOIN FATURA f ON f.id_unidade = uc.id_unidade
JOIN LEITURA l ON l.id_fatura = f.id_fatura
GROUP BY uc.numero_cliente;

-- Compensação de energia
SELECT 
    uc.numero_cliente,
    SUM(ec.quantidade_kwh) as total_compensado,
    SUM(ec.valor_total) as valor_compensado
FROM UNIDADE_CONSUMIDORA uc
JOIN FATURA f ON f.id_unidade = uc.id_unidade
JOIN ENERGIA_COMPENSADA ec ON ec.id_fatura = f.id_fatura
WHERE f.data_emissao BETWEEN :data_inicio AND :data_fim
GROUP BY uc.numero_cliente;
```

### 6.2 Relatórios Principais
- Consumo mensal por unidade
- Histórico de compensação
- Análise de tributos
- Projeção de consumo
- Controle de demanda

## 7. Manutenção e Evolução

### 7.1 Versionamento
- Controle de versão do schema
- Documentação de alterações
- Scripts de migração
- Procedimentos de atualização

### 7.2 Monitoramento
- Alertas de inconsistência
- Métricas de performance
- Logs de erro
- Auditoria de uso

### 7.3 Expansão
- Adição de novas distribuidoras
- Novos tipos de tarifa
- Regras específicas por região
- Integrações externas

## 8. Suporte e Documentação

### 8.1 Documentação Técnica
- Modelos de dados atualizados
- Dicionário de dados
- Procedimentos operacionais
- Guias de troubleshooting

### 8.2 Treinamento
- Manual do usuário
- Procedimentos operacionais
- Casos de uso comuns
- Resolução de problemas

## 9. Contatos e Responsabilidades

### 9.1 Equipe Técnica
- Administrador do banco de dados
- Desenvolvedores responsáveis
- Suporte técnico
- Gestores do sistema

### 9.2 Áreas de Negócio
- Gestores de energia
- Área fiscal/contábil
- Compliance
- Auditoria
