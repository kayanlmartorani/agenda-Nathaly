# P0 — Segurança do Firebase

Este pacote reduz o risco de acesso indevido sem alterar os dados existentes.

## Mudanças

- O primeiro administrador passa a ficar vinculado ao UID gravado em `users/_meta.ownerUid`.
- Usuários novos só podem criar o próprio perfil como `pendente` quando apresentam convite válido e não utilizado.
- Apenas `admin` e `equipe` aprovados acessam dados operacionais.
- Exclusão de clientes fica restrita a `admin`.
- Configurações e gestão de equipe ficam restritas a `admin`.
- Tudo que não estiver explicitamente permitido é negado.

## Implantação segura

1. Fazer backup/exportação do Firestore antes de publicar as regras.
2. Testar em projeto de teste ou emulador:
   - login do admin atual;
   - criação de convite;
   - criação de usuário pendente;
   - aprovação para equipe;
   - leitura e escrita operacional pela equipe;
   - tentativa de pendente ler clientes (deve falhar);
   - tentativa de equipe apagar cliente (deve falhar);
   - tentativa de equipe alterar configuração (deve falhar).
3. Só depois publicar `firestore.rules` na produção.

## Compatibilidade com a instalação atual

Se `users/_meta` já existir sem `ownerUid`, não apague nem recrie esse documento em produção. Os usuários já existentes continuam autenticando pelo documento individual em `users/{uid}`. O novo campo é exigido apenas no bootstrap de uma instalação nova.
