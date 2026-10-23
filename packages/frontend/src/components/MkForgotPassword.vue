<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<MkModalWindow
	ref="dialog"
	class="ogLabAuthTheme"
	:width="370"
	:height="480"
	@close="dialog?.close()"
	@closed="emit('closed')"
	@esc="dialog?.close()"
>
	<template #header>Esqueci minha senha</template>

	<div class="_spacer" style="--MI_SPACER-min: 20px; --MI_SPACER-max: 28px;">
		<form v-if="instance.enableEmail" @submit.prevent="onSubmit">
			<div class="_gaps_m">
				<MkInput v-model="username" type="text" pattern="^[a-zA-Z0-9_]+$" :spellcheck="false" autofocus required>
					<template #label>Nome de usuário</template>
					<template #prefix>@</template>
				</MkInput>

				<MkInput v-model="email" type="email" :spellcheck="false" required>
					<template #label>Endereço de e-mail</template>
					<template #caption>Por favor, insira o endereço de e-mail usado no cadastro de sua conta. Um link para redefinição de senha será enviado para esse endereço.</template>
				</MkInput>

				<MkButton type="submit" rounded :disabled="processing" primary style="margin: 0 auto;">Enviar</MkButton>

				<MkInfo>Caso você não tenha registrado um endereço de e-mail, por favor, entre em contato com o administrador.</MkInfo>
			</div>
		</form>
		<div v-else>
			A recuperação de senha por e-mail não está disponível neste servidor. Entre em contato com a administração para redefinir sua senha.
		</div>
	</div>
</MkModalWindow>
</template>

<script lang="ts" setup>
import { ref, useTemplateRef } from 'vue';
import MkModalWindow from '@/components/MkModalWindow.vue';
import MkButton from '@/components/MkButton.vue';
import MkInput from '@/components/MkInput.vue';
import MkInfo from '@/components/MkInfo.vue';
import * as os from '@/os.js';
import { instance } from '@/instance.js';

const emit = defineEmits<{
	(ev: 'done'): void;
	(ev: 'closed'): void;
}>();

const dialog = useTemplateRef('dialog');

const username = ref('');
const email = ref('');
const processing = ref(false);

async function onSubmit() {
	processing.value = true;
	await os.apiWithDialog('request-reset-password', {
		username: username.value,
		email: email.value,
	});
	emit('done');
	dialog.value?.close();
}
</script>
