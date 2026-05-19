-------------------------------------------------------------------------------
                       DOCUMENTO DE ESCOPO DO SISTEMA (ERP)
-------------------------------------------------------------------------------

-------------------------------------------------------------------------------
1. DIVISÃO DA EQUIPE
-------------------------------------------------------------------------------
- BANCO DE DADOS (BD): Nathan, João
- BACK-END (BK):       Alex, Diogenes
- FRONT-END (FRONT):   Tarek, Wesley

-------------------------------------------------------------------------------
2. MÓDULO: ESTOQUE
-------------------------------------------------------------------------------
[Funções Solicitadas]
- Cadastro do Produto (Nome, código, preço base, categoria).
- Registro de Compras (Entrada de mercadorias no sistema).
- Atualização Automática do Saldo do Estoque (Gatilho após compra/venda).
- Movimentação do Estoque (Histórico de entradas, saídas e ajustes manuais).
- Custo de Material (Preço de custo pago aos fornecedores).
- Edição do Produto (Alterar dados cadastrais).

[Melhorias Sugeridas para Implementar]
- Alerta de Estoque Mínimo: Notificar visualmente quando o produto estiver acabando.
- Código de Barras (SKU): Campo para leitura com leitor óptico ou câmera.
- Motivo da Movimentação: Campo texto obrigatório para saídas manuais (ex: "Avaria", "Perda", "Uso interno").

-------------------------------------------------------------------------------
3. MÓDULO: VENDAS
-------------------------------------------------------------------------------
[Funções Solicitadas]
- Registro de Vendas (Tela de PDV / Checkout).
- Cadastro de Cliente (Dados básicos para faturamento e histórico).
- Controle de Pagamento (Vendedor define a forma de pagamento e o status).
- Histórico de Pagamento (Consultar o que o cliente já pagou e o que deve).
- Alteração de Pedido (Modificar itens antes ou depois de salvar, se permitido).
- Cancelamento do Pedido (Estornar a venda e devolver itens ao estoque).
- Cadastro de Promoções (Regras de desconto que disparam notificação na compra).
- Notícias de Promoções com Imagens (Banners visuais de ofertas vigentes).

[Melhorias Sugeridas para Implementar]
- Múltiplas Formas de Pagamento: Permitir dividir uma venda em (Ex: R$50 Pix + R$50 Cartão).
- Validação de Estoque no PDV: Bloquear a venda se o vendedor tentar vender mais do que tem no saldo físico.
- Vínculo de Vendedor: Registrar qual usuário fez a venda para futuras métricas de comissão.

-------------------------------------------------------------------------------
4. MÓDULO: FINANCEIRO
-------------------------------------------------------------------------------
[Funções Solicitadas]
- Fluxo de Caixa (Registro diário/mensal de entradas e saídas financeiras).
- Visão de Lucro e Perda (DRE - Demonstrativo de Resultado do Exercício simplificado).
- Custo por Temporada de Produto (Análise de variação de preço de custo ao longo do ano).
- Entrega de Produto (Módulo Logística - Marcado como "Olhar Depois").

[Melhorias Sugeridas para Implementar]
- Categorização de Despesas: Separar custos em "Custo de Produto" (variável) e "Custos Operacionais" (Luz, internet, salários).
- Status da Entrega (Para o "Olhar Depois"): Mapear estados simples: "Pendente", "Em Separação", "Enviado", "Entregue".

-------------------------------------------------------------------------------
5. MÓDULO: RELATÓRIOS & CONSULTAS
-------------------------------------------------------------------------------
[Funções Solicitadas]
- Relação de Vendas com Filtro por Tipo (Ex: Vendas balcão, online, atacado).
- Relatório de Estoque (Posição atual, quantidade e valor total investido).
- Relatório de Vendas por Cliente (Quem compra mais, ticket médio).
- Relação Geral de Vendas por Produto (Ranking dos produtos mais vendidos).
- Consulta Rápida de Vendas por Cliente (Filtro direto na tela).

[Melhorias Sugeridas para Implementar]
- Exportação de Dados: Botão para baixar os relatórios em formato CSV/Excel ou PDF para impressão.
- Filtro por Período Global: Todos os relatórios devem obrigatóriamente aceitar filtro de data (Início / Fim).

-------------------------------------------------------------------------------
            REQUISITOS TÉCNICOS & REGRAS DE NEGÓCIO (BACK-END / BD)
-------------------------------------------------------------------------------
1. INTEGRIDADE REFERENCIAL: Se um produto foi vendido, o Banco de Dados não pode 
   permitir sua exclusão física (Delete), apenas desativação (Soft Delete / Inativo), 
   para não quebrar o histórico do financeiro.
2. TRIGGERS / FUNCTIONS: A atualização do saldo de estoque deve ser automatizada 
   via banco de dados ou service no Back-end sempre que o status de um pedido 
   mudar para "Concluído" ou "Cancelado".
3. SEGURANÇA: Definir níveis de acesso. Ex: Vendedor faz vendas e cadastra clientes; 
   Gerente edita estoque e vê relatórios; Administrador mexe no financeiro.
