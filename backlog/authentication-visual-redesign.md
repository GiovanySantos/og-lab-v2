# Authentication flow visual redesign

**Status:** Phase 1 approved — authentication and entry flow

## Summary

Redesign the visual identity and presentation of the authentication experience
for OG Lab, covering sign-in, account registration, and password recovery.
The product owner selected an Old School Cannabis direction and palette B
below. Authentication will use the dark Old School appearance independently
of the active user theme. The product owner approved `oglab-logo.png` to
replace all existing visual Misskey logo/mark placements in the app and as the
source for a favicon. The full logo will appear throughout authentication. Copy is being reviewed
one string at a time. The semantic colors, favicon monogram, typography, and
WCAG 2.2 AA acceptance checks are approved.

## Problem

The authentication flow is a key first impression of OG Lab. Its sign-in,
sign-up, and recovery surfaces should feel like a coherent part of the OG Lab
product rather than isolated default screens.

## Goal

- Establish a recognizable and consistent OG Lab visual experience across the
  authentication flow.
- Make the existing authentication steps clear and usable on desktop and
  mobile.
- Preserve the existing authentication behavior and account policies.

## Selected identity direction

Use an Old School Cannabis visual direction with worn orange and black,
complemented by the aged cream, dark leaf green, and ochre found in the approved
logo reference: [`oglab-logo.png`](./oglab-logo.png).

### Selected starting palette

These swatches were selected as palette B. The product owner approved the
listed roles as a baseline for design and prototyping; final theme-token values
must still pass contrast and accessibility review.

| Color | HEX | Approved starting role |
| --- | --- | --- |
| Worn orange | `#D87536` | Brand accent and primary actions |
| Worn black | `#24211D` | Dark theme canvas, text, and outlines |
| Aged parchment | `#E7D7B8` | Light theme canvas and dark-theme foreground |
| Leaf green | `#526647` | Secondary brand accent, reflecting the logo |
| Aged ochre | `#B99558` | Decorative accent, reflecting the logo |

Authentication uses the dark theme only. Use `#24211D` as the starting
background and `#E7D7B8` for primary text. The proposed orange accent
`#D87536` has a 4.97:1 contrast ratio against the dark background and should
use dark text (`#24211D`, also 4.97:1) when used as a filled control. Final
token use must still pass the approved WCAG criteria.

Approved semantic colors for the dark authentication surface:

| Semantic role | HEX | Contrast against `#24211D` |
| --- | --- | --- |
| Link | `#7DB8C9` | 7.30:1 |
| Success | `#9BC28B` | 8.00:1 |
| Warning | `#E3BB73` | 8.87:1 |
| Error | `#F28B82` | 6.71:1 |

Ratios use the WCAG relative-luminance formula. Recheck the final rendered
colors and focus indicators during implementation.

### Authentication theme behavior

- Authentication surfaces use the OG Lab identity independently of the active
  user-selected theme.
- Use the dark Old School appearance for authentication.
- Display the full approved logo/wordmark throughout the authentication flow.
- This must not change or overwrite the user's saved theme; the selected theme
  continues to apply outside authentication.

### Logo and asset inventory to approve

Use the supplied `oglab-logo.png` as the approved visual source. Replace all
existing visual Misskey logo/mark placements in the app with OG Lab branding,
including authentication, welcome, and QR surfaces. Derive a favicon from the
approved logo.

Retain the full wording “AUTHENTIC OG LAB BY FPUNK” wherever the full wordmark
is used. Display the full logo/wordmark throughout authentication. Produce a
favicon from the approved image and validate its legibility at 16, 32, and 48
pixels. Use a simplified “OG” monogram adapted from the approved lettering.
The favicon is the only app-icon deliverable in this scope.

## Scope

Define and apply this visual direction to the existing authentication surfaces
and their relevant states, and replace existing Misskey logo/mark placements
in the app:

- Sign-in, including username, password, TOTP, and passkey steps.
- Sign-up, including server rules, registration fields, and completion or
  pending-email states.
- Registration rules/terms modal and its agreement flow.
- Password recovery and password reset.
- Supporting visual states such as loading, validation feedback, errors, and
  success messages.

## Out of scope

- Changes to authentication APIs, validation rules, account policies, or
  registration eligibility.
- Changes to sign-in methods or the order and logic of authentication steps.
- Changes to email delivery, password reset tokens, or account security.
- Broader branding changes outside authentication and the approved replacement
  of all existing Misskey logo/mark placements and favicon.
- A global replacement of the app's built-in default themes or assets.
- Rewriting upstream product names or other user-facing copy outside the
  approved full logo wordmark.

## Product requirements

1. The in-scope screens should use a coherent OG Lab visual language.
2. Existing authentication steps, validation, and outcomes must remain
   available and function as they do today.
3. The experience should remain usable on mobile and desktop viewports.
4. Form labels, feedback, focus states, and controls should remain clear and
   accessible, meeting WCAG 2.2 AA as defined in Acceptance criteria.
5. Instance-specific settings and existing localization should continue to be
   respected.
6. Authentication surfaces should use the dark OG Lab palette even when a
   user-selected theme is active, without changing that saved theme elsewhere.

## Acceptance criteria

- Authentication and branded logo placements are checked at mobile widths
  starting at 320 CSS pixels and at desktop widths up to 1920 CSS pixels.
- Sign-in with password, TOTP, and passkey; sign-up; password recovery; and
  reset flows retain their existing behavior.
- Text contrast meets WCAG 2.2 AA: at least 4.5:1 for normal text and 3:1 for
  large text.
- Focus indicators and meaningful non-text UI graphics meet at least 3:1
  contrast against adjacent colors; states are not communicated by color alone.
- Every existing in-app visual Misskey logo/mark placement is replaced by OG
  Lab artwork, including welcome and QR screens; unrelated upstream names and
  copy are not rewritten unless part of the approved logo.
- On the visitor landing screen, show the approved OG Lab image and the exact
  heading `Bem vindo ao OG Lab`; remove the two yellow notices and show only
  `Criar uma conta` and `Entrar` as flow buttons.
- Preserve existing signup/sign-in destinations and invite/federation
  behavior; remove `Buscar outra instância` from this screen without changing
  server discovery elsewhere.
- Authentication keeps the dark OG Lab appearance even when another theme is
  selected, and does not overwrite the saved theme used elsewhere.
- The favicon remains identifiable at 16, 32, and 48 pixels.

## Success measures

- Users can complete sign-in, sign-up, and password recovery without a
  regression in the existing flow.
- The authentication surfaces are visually consistent with the agreed OG Lab
  identity.
- Usability and visual quality are reviewed across mobile and desktop layouts.

Quantitative targets and baseline measurements are TBD.

## Localization and staged migration

OG Lab is intended to serve Brazilian users in Brazilian Portuguese only. The
product direction is to remove Crowdin and runtime i18n dependencies
gradually, replacing translated UI strings with direct Brazilian Portuguese
text as each area is migrated.

- Phase 1 is the entry/authentication flow described in this PRD.
- For in-scope migrated screens, use direct Brazilian Portuguese text in the
  frontend; do not add new locale keys or depend on Crowdin for those screens.
- Keep existing locale files, Crowdin configuration, and shared i18n runtime
  while unmigrated parts of the application still depend on them.
- Do not remove or disable the global localization infrastructure until a
  later migration phase verifies that no remaining application surface relies
  on it.
- Do not manually edit non-source locale YAML files as part of this staged
  migration.
- No phase may depend on access, authorization, project changes, or manual
  work from upstream Misskey/Crowdin administrators.
- Translate existing functional messages faithfully into Brazilian
  Portuguese when converting a screen; preserve meaning, placeholders,
  accessibility, and behavior. The product owner-approved copy changes below
  take precedence over the existing translations.

### First screen: visitor landing page

The product owner approved the following changes for the first screen shown in
the supplied screenshot:

- Replace the green Misskey “Mi” mark with the approved OG Lab image at
  `backlog/oglab-logo.png`.
- Replace the heading `og-lab` with the exact text `Bem vindo ao OG Lab`.
- Remove the two yellow announcement/warning notices shown in the screenshot.
- Show only these two primary flow buttons, with the exact labels:
  - `Criar uma conta`
  - `Entrar`
- Remove the other buttons currently shown on this screen, including
  `Buscar outra instância`.

These are presentation and navigation-surface changes only. Preserve the
existing signup and sign-in behavior, invite requirements, and server
federation settings. Do not infer additional copy changes or remove content
outside the screenshot items above.

### Authentication and signup copy: reviewed strings

- Username field placeholder: keep the current text `Nome de usuário`.
- Continue button after username: keep the current text `Continuar`.
- Passwordless sign-in button: change `Entrar com Passkey` to
  `Entrar com chave de acesso`.
- Password-step greeting: keep `Bem-vindo de volta, {name}`.
- Password field placeholder: keep the current text `Senha`.
- Continue button after password and on later authentication steps: keep
  `Continuar`, since the flow may still require another factor.
- Password recovery link: approve `Esqueci minha senha` as the intended
  Brazilian Portuguese wording; implement and validate it through an
  OG-Lab-controlled, repository-local localization path.
- Use `Esqueci minha senha` for both the password recovery link and the
  recovery modal title (shared wording).
- Password recovery email label: keep `Endereço de e-mail`.
- Password recovery help text: keep `Por favor, insira o endereço de e-mail
  usado no cadastro de sua conta. Um link para redefinição de senha será
  enviado para esse endereço.`
- Password recovery no-email help text: keep `Caso você não tenha registrado
  um endereço de e-mail, por favor, entre em contato com o administrador.`
- When email recovery is disabled: change the message to `A recuperação de
  senha por e-mail não está disponível neste servidor. Entre em contato com a
  administração para redefinir sua senha.`
- Password recovery submit button: keep `Enviar`.
- Password reset page title: keep `Redefinir senha`.
- Password reset field label: keep `Nova senha`.
- Password reset submit button: keep `Salvar`.
- Signup password confirmation label: keep `Senha (Digite novamente)`.
- Signup email field label: change `Endereço de e-mail` to `E-mail`.
- Signup username field label: keep `Nome de usuário`.
- Signup username permanence caption: change `Isso não pode ser alterado.`
  to `O nome de usuário não poderá ser alterado depois.`
- Signup username help tooltip: keep `O nome para identificar exclusivamente
  a sua conta no servidor. Pode conter letras (az, AZ), números (0~9) e
  sublinhados (_). O nome de usuário não pode ser alterado posteriormente.`
- Signup email help text: keep `Por favor, insira o seu endereço de e-mail.
  Ele não será divulgado.`
- Signup password field label: keep `Senha`.
- Signup weak-password feedback: keep `Senha fraca`.
- Signup medium-password feedback: keep `Senha normal`.
- Signup strong-password feedback: keep `Senha forte`.
- Signup matching-password feedback: keep `As senhas coincidem`.
- Signup mismatched-password feedback: keep `As senhas não coincidem`.
- Signup email validation loading state: keep `Verificando...`.
- Signup email available state: keep `Disponível`.
- Signup email already-used validation: keep `O endereço de e-mail informado
  já está sendo utilizado`.
- Signup invalid email format feedback: keep the current text
  `Formado de e-mail inválido`.
- Signup form submit button: keep the current text `começar`.
- Registration terms section/link label: change `Observações importantes` to
  `Termo de adesão do OG Lab`.
- Registration terms consent label: change `Concordar` to `Li e concordo`.
- Registration rules modal cancel button: keep `Cancelar`.
- Registration consent prompt: change the current text to
  `Leia e aceite todos os itens para continuar.`
- Invitation-only signup field label: keep `Código de convite`.
- TOTP token field label: change the current `Símbolo (Autenticação de dois
  fatores)` to `Código de autenticação (Autenticação de dois fatores)`.
- TOTP screen backup-code toggle: keep `Usar códigos de “backup”`.
- TOTP screen authenticator-code toggle: change `Digite a senha de uso único`
  to `Usar código do autenticador`.
- Passkey instruction: keep `Por favor, siga as instruções do seu navegador
  ou dispositivo para utilizar uma chave de acesso.`
- Passkey retry button: keep `Tente novamente`.

The approved copy decisions above are the only copy changes in scope. All
other signup, validation, recovery, and completion text stays as it is today.
During Phase 1, these unchanged messages are still rendered as Brazilian
Portuguese direct text rather than loaded through i18n.

### Registration modal: rules and agreement

The product owner approved the following changes for the registration rules
modal shown in the supplied screenshot:

- Remove both yellow notices (invitation-only registration and disabled
  federation) from this modal. Do not change the underlying invitation or
  federation settings or enforcement.
- Replace `Para cadastrar-se no servidor, você precisa ler e concordar como seguinte:`
  with the exact text `Leia nossas regras antes de se registrar`.
- Remove `Por favor, leia essa informação importante.`
- Rewrite the agreement/terms content as OG Lab-owned content.
- Replace the current Misskey terms/information link with a page hosted by OG
  Lab, not a Misskey page.

For the initial implementation, use clearly marked Lorem Ipsum placeholder
content and link to a local OG Lab page/route, not to a Misskey-hosted page or
external Misskey URL. The exact route can be chosen during implementation.
Open the local page in a new browser tab.
Replace the placeholder with approved OG Lab terms before treating the page as
final or production-ready. Keep the consent requirement and signup gating
behavior intact. Do not imply that placeholder text is legally valid or
approved, and do not silently add legal obligations or privacy statements.

## Remaining product-owner decisions

- Authoritative OG Lab terms/agreement text and confirmation that it is
  approved for publication; Lorem Ipsum is temporary only.
- Define the repository-local, OG-Lab-controlled approach for delivering
  Brazilian Portuguese across application areas outside Phase 1.

No other field captions, validation messages, recovery text, or registration
completion copy require review for this identity update; keep their current
wording unchanged.

## Existing implementation notes

- Built-in light and dark theme bases live in
  `packages/frontend-shared/themes/_light.json5` and
  `packages/frontend-shared/themes/_dark.json5`; both currently use the
  Misskey green accent `#86b300`.
- The frontend theme manager compiles built-in and user themes. Brand defaults
  should be designed around that system and must not discard user-selected
  themes.
- Existing identity assets include `packages/frontend/assets/misskey.svg` and
  browser/app icons under `packages/backend/assets/`. The approved
  `backlog/oglab-logo.png` replaces existing visual Misskey logo/mark
  placements in the app and serves as the source for the favicon. This does not
  imply replacing upstream product names or unrelated assets.
- Known frontend mark usages include
  `packages/frontend/src/pages/welcome.entrance.simple.vue`,
  `packages/frontend/src/pages/welcome.entrance.classic.vue`, and
  `packages/frontend/src/pages/qr.show.vue`; replace these along with any other
  existing in-app visual mark usage found during implementation.
- Keep the existing sans-serif typography; no added vintage texture is planned.
- The favicon uses a simplified “OG” monogram derived from the approved logo;
  validate at 16, 32, and 48 pixels.

## Current implementation references

- `packages/frontend/src/components/MkSignin.vue` and related sign-in step
  components.
- `packages/frontend/src/components/MkSignupDialog.vue` and
  `MkSignupDialog.form.vue`.
- `packages/frontend/src/components/MkForgotPassword.vue`.
- `packages/frontend/src/pages/reset-password.vue`.
- `packages/frontend/src/pages/signup-complete.vue`.
- `packages/frontend/src/components/MkSignupDialog.rules.vue` and related
  rules/terms content and links.
