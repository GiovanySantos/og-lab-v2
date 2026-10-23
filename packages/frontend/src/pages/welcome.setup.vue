<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<PageWithAnimBg class="ogLabAuthTheme">
	<div :class="$style.formContainer">
		<div :class="$style.form" class="_panel">
			<div :class="$style.banner">
				<img :src="ogLabLogo" alt="Logotipo AUTHENTIC OG LAB BY FPUNK" :class="$style.logo">
				<div :class="$style.version">v{{ version }}</div>
			</div>
			<div style="padding: 16px 32px 32px 32px;">
				<form v-if="!accountCreated" class="_gaps_m" @submit.prevent="createAccount()">
					<div style="text-align: center;" class="_gaps_s">
						<div><b>A instalação foi concluída!</b></div>
						<div>Para começar, crie a conta de administrador.</div>
					</div>
					<MkInput v-model="setupPassword" type="password" data-testid="admin-initial-password">
						<template #label>Senha inicial de configuração <div v-tooltip:dialog="'Se você instalou o servidor por conta própria, use a senha definida no arquivo de configuração.\nSe usa um serviço de hospedagem, use a senha fornecida por ele.\nSe nenhuma senha foi definida, deixe em branco para continuar.'" class="_button _help"><i class="ti ti-help-circle"></i></div></template>
						<template #prefix><i class="ti ti-lock"></i></template>
					</MkInput>
					<MkInput v-model="username" pattern="^[a-zA-Z0-9_]{1,20}$" :spellcheck="false" required data-testid="admin-username">
						<template #label>Nome de usuário <div v-tooltip:dialog="'Um nome que identifica sua conta neste servidor. Você pode usar letras (a~z, A~Z), números (0~9) ou sublinhados (_). O nome de usuário não pode ser alterado depois.'" class="_button _help"><i class="ti ti-help-circle"></i></div></template>
						<template #prefix>@</template>
						<template #suffix>@{{ host }}</template>
					</MkInput>
					<MkInput v-model="password" type="password" data-testid="admin-password">
						<template #label>Senha</template>
						<template #prefix><i class="ti ti-lock"></i></template>
					</MkInput>
					<div>
						<MkButton gradate large rounded :disabled="accountCreating" data-testid="admin-ok" style="margin: 0 auto;" type="submit">
							{{ accountCreating ? 'Processando...' : 'Avançar' }}<MkEllipsis v-if="accountCreating"/>
						</MkButton>
					</div>
				</form>
				<div v-else-if="step === 0" class="_gaps_m">
					<div style="text-align: center;" class="_gaps_s">
						<div><b>A conta de administrador foi criada!</b></div>
					</div>
					<MkButton gradate large rounded data-testid="next" style="margin: 0 auto;" @click="step++">
						Avançar
					</MkButton>
				</div>
				<div v-else-if="step === 1" class="_gaps_m">
					<div style="text-align: center;" class="_gaps_s">
						<div style="font-size: 120%;"><b>Configurações do servidor</b></div>
						<div>Este assistente facilita a configuração ideal do servidor.</div>
						<div>As configurações feitas aqui podem ser alteradas depois.</div>
					</div>

					<Suspense>
						<template #default>
							<MkServerSetupWizard :token="token!" @finished="onWizardFinished"/>
						</template>
						<template #fallback>
							<MkLoading/>
						</template>
					</Suspense>

					<MkButton rounded style="margin: 0 auto;" @click="skipSettings">
						Pular configurações
					</MkButton>
				</div>
				<div v-else-if="step === 2" class="_gaps_m">
					<div style="text-align: center;" class="_gaps_s">
						<div><b>A configuração foi concluída!</b></div>
						<div>Obrigado pelo seu tempo. Agora que tudo está pronto, você já pode começar a usar o servidor.</div>
						<div>As configurações do servidor podem ser alteradas no “Painel de controle”.</div>
					</div>
					<div class="_buttonsCenter">
						<MkButton gradate large rounded data-testid="next" style="margin: 0 auto;" @click="finish">
							Começar
						</MkButton>
					</div>
				</div>
			</div>
		</div>
	</div>
</PageWithAnimBg>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import { host, version } from '@@/js/config.js';
import MkButton from '@/components/MkButton.vue';
import MkInput from '@/components/MkInput.vue';
import * as os from '@/os.js';
import { misskeyApi } from '@/utility/misskey-api.js';
import { i18n } from '@/i18n.js';
import { login } from '@/accounts.js';
import MkServerSetupWizard from '@/components/MkServerSetupWizard.vue';
import ogLabLogo from '/client-assets/oglab-logo.png';

const username = ref('');
const password = ref('');
const setupPassword = ref('');
const accountCreating = ref(false);
const accountCreated = ref(false);
const step = ref(0);

let token: string | null = null;

function createAccount() {
	if (accountCreating.value) return;
	accountCreating.value = true;

	const _close = os.waiting();

	misskeyApi('admin/accounts/create', {
		username: username.value,
		password: password.value,
		setupPassword: setupPassword.value === '' ? null : setupPassword.value,
	}).then(res => {
		token = res.token;
		accountCreated.value = true;
	}).catch((err) => {
		accountCreating.value = false;

		let title = i18n.ts.somethingHappened;
		let text = err.message + '\n' + err.id;

		if (err.code === 'ACCESS_DENIED') {
			title = i18n.ts.permissionDeniedError;
			text = i18n.ts.operationForbidden;
		} else if (err.code === 'INCORRECT_INITIAL_PASSWORD') {
			title = i18n.ts.permissionDeniedError;
			text = i18n.ts.incorrectPassword;
		}

		os.alert({
			type: 'error',
			title,
			text,
		});
	}).finally(() => {
		_close();
	});
}

function onWizardFinished() {
	step.value++;
}

function skipSettings() {
	step.value++;
}

function finish() {
	if (token == null) return;
	login(token);
}
</script>

<style lang="scss" module>
.formContainer {
	min-height: 100svh;
	padding: 32px 32px 64px 32px;
	box-sizing: border-box;
	background: var(--MI_THEME-bg);
	align-content: center;
}

.form {
	position: relative;
	z-index: 10;
	border-radius: var(--MI-radius);
	box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
	overflow: clip;
	max-width: 550px;
	margin: 0 auto;
}

.banner {
	padding: 24px 16px 8px;
	text-align: center;
}

.logo {
	display: block;
	width: 180px;
	max-width: 100%;
	height: auto;
	margin: 0 auto;
}

.version {
	margin-top: 8px;
	font-size: 70%;
	opacity: 0.7;
}
</style>
