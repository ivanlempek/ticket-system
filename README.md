# Fluxo resumido

1 - Acesso → CDN → API Gateway.

2 - /join-queue → Queue Service → retorna TicketToken (queued, position=N).

3 - Worker observa disponibilidade de ingressos; quando houver, muda token para lobby e envia via WebSocket/Server-Sent Events.

4 - Navegador com token lobby pode chamar /reserve.

5 - Ticket Inventory Service executa CAS (UPDATE stock SET available=available-1 WHERE id=? AND available>0).

6 - Reserva gerada (TTL = 5 min) → usuário paga → Order Service confirma.

7 - Event Bus dispara TicketSold; painel Analytics atualiza em tempo real.

8 - Token ou reserva expira → Worker lança rollback e volta ingresso ao pool.
