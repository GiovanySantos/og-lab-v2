<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<div>
	<div :class="$style.banner">
		<img :src="ogLabLogo" alt="AUTHENTIC OG LAB BY FPUNK" :class="$style.logo"/>
	</div>
	<div class="_spacer" style="--MI_SPACER-min: 20px; --MI_SPACER-max: 28px;">
		<div class="_gaps_m">
			<div :class="$style.intro">
				<h2 :class="$style.title">Leia nossas regras antes de se registrar</h2>
			</div>

			<MkFolder v-if="availableServerRules" :defaultOpen="true">
				<template #label>Regras do servidor</template>
				<template #suffix><i v-if="agreeServerRules" class="ti ti-check" style="color: var(--MI_THEME-success)"></i></template>

				<ol class="_gaps_s" :class="$style.rules">
					<li v-for="item in instance.serverRules" :class="$style.rule"><div :class="$style.ruleText" v-html="item"></div></li>
				</ol>

				<MkSwitch :modelValue="agreeServerRules" style="margin-top: 16px;" @update:modelValue="updateAgreeServerRules">Li e concordo</MkSwitch>
			</MkFolder>

			<MkFolder v-if="availableTos || availablePrivacyPolicy" :defaultOpen="true">
				<template #label>{{ tosPrivacyPolicyLabel }}</template>
				<template #suffix><i v-if="agreeTosAndPrivacyPolicy" class="ti ti-check" style="color: var(--MI_THEME-success)"></i></template>
				<div class="_gaps_s">
					<div v-if="availableTos"><a :href="instance.tosUrl ?? undefined" class="_link" target="_blank" rel="noopener noreferrer">Termos de serviço <i class="ti ti-external-link"></i></a></div>
					<div v-if="availablePrivacyPolicy"><a :href="instance.privacyPolicyUrl ?? undefined" class="_link" target="_blank" rel="noopener noreferrer">Política de privacidade <i class="ti ti-external-link"></i></a></div>
				</div>

				<MkSwitch :modelValue="agreeTosAndPrivacyPolicy" style="margin-top: 16px;" @update:modelValue="updateAgreeTosAndPrivacyPolicy">Li e concordo</MkSwitch>
			</MkFolder>

			<MkFolder :defaultOpen="true">
				<template #label>Termo de adesão do OG Lab</template>
				<template #suffix><i v-if="agreeNote" class="ti ti-check" style="color: var(--MI_THEME-success)"></i></template>

				<a href="/oglab-terms" class="_link" target="_blank" rel="noopener noreferrer">Ler o termo de adesão do OG Lab <i class="ti ti-external-link"></i></a>

				<MkSwitch :modelValue="agreeNote" style="margin-top: 16px;" data-testid="signup-rules-notes-agree" @update:modelValue="updateAgreeNote">Li e concordo</MkSwitch>
			</MkFolder>

			<div v-if="!agreed" :class="$style.consentPrompt">Leia e aceite todos os itens para continuar.</div>

			<div class="_buttonsCenter">
				<MkButton inline rounded @click="emit('cancel')">Cancelar</MkButton>
				<MkButton inline primary rounded gradate :disabled="!agreed" data-testid="signup-rules-continue" @click="emit('done')">Continuar <i class="ti ti-arrow-right"></i></MkButton>
			</div>
		</div>
	</div>
</div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';
import { instance } from '@/instance.js';
import ogLabLogo from '/client-assets/oglab-logo.png';
import MkButton from '@/components/MkButton.vue';
import MkFolder from '@/components/MkFolder.vue';
import MkSwitch from '@/components/MkSwitch.vue';
import * as os from '@/os.js';

const availableServerRules = instance.serverRules.length > 0;
const availableTos = instance.tosUrl != null && instance.tosUrl !== '';
const availablePrivacyPolicy = instance.privacyPolicyUrl != null && instance.privacyPolicyUrl !== '';

const agreeServerRules = ref(false);
const agreeTosAndPrivacyPolicy = ref(false);
const agreeNote = ref(false);

const agreed = computed(() => {
	return (!availableServerRules || agreeServerRules.value) && ((!availableTos && !availablePrivacyPolicy) || agreeTosAndPrivacyPolicy.value) && agreeNote.value;
});

const emit = defineEmits<{
	(ev: 'cancel'): void;
	(ev: 'done'): void;
}>();

const tosPrivacyPolicyLabel = computed(() => {
	if (availableTos && availablePrivacyPolicy) {
		return 'Termos de serviço e política de privacidade';
	} else if (availableTos) {
		return 'Termos de serviço';
	} else if (availablePrivacyPolicy) {
		return 'Política de privacidade';
	} else {
		return '';
	}
});

async function updateAgreeServerRules(v: boolean) {
	if (v) {
		const confirm = await os.confirm({
			type: 'question',
			title: 'Confirmar aceite',
			text: 'Li e concordo com as regras do servidor.',
			okText: 'Concordo',
			cancelText: 'Cancelar',
			ogLabAuthTheme: true,
		});
		if (confirm.canceled) return;
		agreeServerRules.value = true;
	} else {
		agreeServerRules.value = false;
	}
}

async function updateAgreeTosAndPrivacyPolicy(v: boolean) {
	if (v) {
		const confirm = await os.confirm({
			type: 'question',
			title: 'Confirmar aceite',
			text: `Li e concordo com ${tosPrivacyPolicyLabel.value.toLocaleLowerCase('pt-BR')}.`,
			okText: 'Concordo',
			cancelText: 'Cancelar',
			ogLabAuthTheme: true,
		});
		if (confirm.canceled) return;
		agreeTosAndPrivacyPolicy.value = true;
	} else {
		agreeTosAndPrivacyPolicy.value = false;
	}
}

async function updateAgreeNote(v: boolean) {
	if (v) {
		const confirm = await os.confirm({
			type: 'question',
			title: 'Confirmar aceite',
			text: 'Li e concordo com o termo de adesão do OG Lab.',
			okText: 'Concordo',
			cancelText: 'Cancelar',
			ogLabAuthTheme: true,
		});
		if (confirm.canceled) return;
		agreeNote.value = true;
	} else {
		agreeNote.value = false;
	}
}
</script>

<style lang="scss" module>
.banner {
	padding: 12px 16px;
	text-align: center;
	background-color: var(--MI_THEME-panel);
	border-bottom: 1px solid var(--MI_THEME-divider);
}

.logo {
	display: block;
	width: min(180px, 60vw);
	max-height: 116px;
	margin: 0 auto;
	object-fit: contain;
}

.intro {
	text-align: center;
}

.title {
	margin: 0;
	font-size: 1.2rem;
	line-height: 1.4;
}

.consentPrompt {
	text-align: center;
	color: var(--MI_THEME-warn);
}

.rules {
	counter-reset: item;
	list-style: none;
	padding: 0;
	margin: 0;
}

.rule {
	display: flex;
	gap: 8px;
	word-break: break-word;

	&::before {
		flex-shrink: 0;
		display: flex;
		position: sticky;
		top: calc(var(--MI-stickyTop, 0px) + 8px);
		counter-increment: item;
		content: counter(item);
		width: 32px;
		height: 32px;
		line-height: 32px;
		background-color: var(--MI_THEME-accentedBg);
		color: var(--MI_THEME-accent);
		font-size: 13px;
		font-weight: bold;
		align-items: center;
		justify-content: center;
		border-radius: 999px;
	}
}

.ruleText {
	padding-top: 6px;
}
</style>
