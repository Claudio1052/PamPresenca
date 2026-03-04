# BioCheck — Check-in Biométrico
### GitHub Pages + Supabase + WebAuthn FIDO2

---

## 📁 Arquivos

```
biocheck/
├── index.html            ← Página principal do check-in (GitHub Pages usa este)
├── admin.html            ← Painel administrativo + exportação CSV
├── supabase_schema.sql   ← Execute UMA vez no Supabase SQL Editor
└── README.md
```

---

## 🚀 Deploy

### Passo 1 — Supabase: criar as tabelas

1. Acesse [supabase.com](https://supabase.com) → seu projeto
2. Vá em **SQL Editor** → cole o conteúdo de `supabase_schema.sql` → **Run**

### Passo 2 — GitHub Pages

```bash
git init
git add .
git commit -m "feat: BioCheck"
git remote add origin https://github.com/SEU-USUARIO/biocheck.git
git push -u origin main
```

No GitHub: **Settings → Pages → Branch: main → / (root) → Save**

URL de acesso: `https://SEU-USUARIO.github.io/biocheck/`

> ⚠️ **WebAuthn exige HTTPS** — o GitHub Pages já fornece automaticamente.

---

## 🔐 Supabase configurado

| Parâmetro | Valor |
|---|---|
| Project URL | `https://cqjcajxjtevvquouengk.supabase.co` |
| Anon Key | embutida nos arquivos HTML |

As credenciais já estão **hardcoded** em `index.html` e `admin.html`.
Nenhuma configuração extra é necessária ao abrir as páginas.

---

## 🗃️ Tabelas Supabase

| Tabela | Descrição |
|---|---|
| `participants` | Perfil do usuário, chave pública WebAuthn, status de presença |
| `auth_log` | Auditoria completa: register, verify_ok, verify_fail |

---

## 📋 Requisitos do dispositivo

| Item | Requisito |
|---|---|
| Sensor | Touch ID, Face ID, Windows Hello ou chave FIDO2 |
| Navegador | Chrome 67+, Safari 14+, Edge 18+, Firefox 60+ |
| Protocolo | **HTTPS obrigatório** (GitHub Pages fornece automaticamente) |
