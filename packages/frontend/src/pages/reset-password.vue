<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<PageWithHeader class="ogLabAuthTheme" :actions="headerActions" :tabs="headerTabs">
	<div v-if="token" class="_spacer" style="--MI_SPACER-w: 700px; --MI_SPACER-min: 16px; --MI_SPACER-max: 32px;">
		<div class="_panel" :class="$style.form">
			<div class="_gaps_m">
				<img :src="ogLabLogo" alt="Logotipo AUTHENTIC OG LAB BY FPUNK" :class="$style.logo">
				<MkInput v-model="password" ariaLabel="Nova senha" type="password">
					<template #prefix><i class="ti ti-lock"></i></template>
					<template #label>Nova senha</template>
				</MkInput>

				<MkButton rounded primary style="margin: 0 auto;" @click="save">Salvar</MkButton>
			</div>
		</div>
	</div>
</PageWithHeader>
</template>

<script lang="ts" setup>
import { defineAsyncComponent, onMounted, ref, computed } from 'vue';
import MkInput from '@/components/MkInput.vue';
import MkButton from '@/components/MkButton.vue';
import * as os from '@/os.js';
import { definePage } from '@/page.js';
import { mainRouter } from '@/router.js';
import ogLabLogo from '/client-assets/oglab-logo.png';

const props = defineProps<{
	token?: string;
}>();

const password = ref('');

async function save() {
	if (props.token == null) return;
	await os.apiWithDialog('reset-password', {
		token: props.token,
		password: password.value,
	});
	mainRouter.push('/');
}

onMounted(async () => {
	if (props.token == null) {
		const { dispose } = await os.popupAsyncWithDialog(import('@/components/MkForgotPassword.vue').then(x => x.default), {}, {
			closed: () => dispose(),
		});
		mainRouter.push('/');
	}
});

const headerActions = computed(() => []);

const headerTabs = computed(() => []);

definePage(() => ({
	title: 'Redefinir senha',
	icon: 'ti ti-lock',
}));
</script>

<style lang="scss" module>
.form {
	max-width: 500px;
	margin: 0 auto;
	padding: calc(var(--MI-margin) * 2);
	box-sizing: border-box;
}

.logo {
	display: block;
	width: 100%;
	max-width: 220px;
	height: auto;
	margin: 0 auto;
}
</style>
