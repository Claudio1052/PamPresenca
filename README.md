# BioCheck — Check-in Biométrico
### GitHub Pages + Supabase + WebAuthn FIDO2

---

## 📁 Arquivos do projeto

```
biocheck/
├── checkin.html          ← Página de check-in (abrir no evento)
├── admin.html            ← Painel administrativo + exportação CSV
├── supabase_schema.sql   ← Execute uma vez no Supabase SQL Editor
└── README.md
```

---

## 🚀 Deploy em 5 passos

### 1. Criar projeto no Supabase

1. Acesse [supabase.com](https://supabase.com) → **New Project**
2. Anote a **Project URL** e a **anon/public key** em **Settings → API**
3. Abra o **SQL Editor** e cole o conteúdo de `supabase_schema.sql` → **Run**

### 2. Publicar no GitHub Pages

```bash
# Crie um repositório público no GitHub, ex: biocheck
git init
git add checkin.html admin.html supabase_schema.sql README.md
git commit -m "feat: BioCheck inicial"
git remote add origin https://github.com/SEU-USUARIO/biocheck.git
git push -u origin main
```

No GitHub: **Settings → Pages → Branch: main → / (root) → Save**

Sua URL será: `https://SEU-USUARIO.github.io/biocheck/`

> **HTTPS obrigatório** — WebAuthn só funciona em HTTPS ou `localhost`.
> O GitHub Pages já fornece HTTPS automaticamente. ✓

### 3. Configurar o Supabase na interface

Ao abrir `checkin.html` ou `admin.html` pela primeira vez, um painel amarelo
pedirá sua **Project URL** e **Anon Key**. As credenciais são salvas no
`localStorage` do navegador (não são enviadas a nenhum servidor externo).

---

## 🔐 Como funciona

```
Usuário preenche formulário → toca o sensor biométrico
        ↓
Navegador gera par de chaves no TPM / Secure Enclave
        ↓
Chave PRIVADA → permanece no dispositivo (nunca transmitida)
Chave PÚBLICA → gravada no Supabase via REST API (HTTPS)
        ↓
Check-in: Supabase envia credentialId → dispositivo assina
        com chave privada → UPDATE presente=true + checkin_at
```

---

## 🗃️ Tabelas Supabase

| Tabela | Descrição |
|---|---|
| `participants` | Perfil, chave pública, status de presença |
| `auth_log` | Auditoria de todos os eventos (register / verify_ok / verify_fail) |
| `v_presenca` | View de consulta para o painel admin |

---

## ⚙️ Configuração avançada

### Hardcode das credenciais (evita o painel de config)

Em `checkin.html` e `admin.html`, substitua o bloco de init por:

```javascript
// Substitui initSupabase() — credenciais fixas
sb = window.supabase.createClient(
  'https://SEU-PROJETO.supabase.co',
  'sua-anon-key-aqui'
);
```

### Restringir o painel admin

Crie uma política RLS mais restritiva no Supabase para
`SELECT` em `participants`, exigindo um papel autenticado.
Ou adicione uma senha simples no `admin.html` antes de exibir o conteúdo.

---

## 📋 Requisitos

| Item | Requisito |
|---|---|
| Dispositivo | Touch ID, Face ID, Windows Hello ou chave FIDO2 |
| Navegador | Chrome 67+, Safari 14+, Firefox 60+, Edge 18+ |
| Protocolo | **HTTPS obrigatório** (GitHub Pages fornece automaticamente) |
| Backend | Conta gratuita no Supabase (500 MB, sem servidor para gerenciar) |
