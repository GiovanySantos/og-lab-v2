<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<PageWithHeader class="ogLabAuthTheme" :actions="headerActions" :tabs="headerTabs">
	<div class="_spacer" style="--MI_SPACER-w: 700px; --MI_SPACER-min: 20px; --MI_SPACER-max: 32px;">
		<section :class="$style.content">
			<div :class="$style.notice" role="note">
				<strong>CONTEÚDO PROVISÓRIO</strong>
				<p>Este texto é um placeholder e não representa termos oficiais aprovados pelo OG Lab.</p>
			</div>
			<h1>Termo de adesão do OG Lab</h1>
			<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer non justo a erat luctus consequat. Curabitur vitae sem sed libero posuere feugiat.</p>
			<p>Praesent euismod, metus at feugiat consequat, nibh sem luctus erat, sed tincidunt justo neque vitae velit. Donec vel nisi ac ante volutpat gravida.</p>
			<p>Fusce consequat, sapien sed luctus aliquet, nunc justo tincidunt erat, sed posuere magna neque vitae lectus. Nulla facilisi.</p>
		</section>
	</div>
</PageWithHeader>
</template>

<script lang="ts" setup>
import { computed } from 'vue';
import { definePage } from '@/page.js';

const headerActions = computed(() => []);
const headerTabs = computed(() => []);

definePage(() => ({
	title: 'Termo de adesão do OG Lab',
	icon: 'ti ti-file-description',
}));
</script>

<style lang="scss" module>
.content {
	padding: 24px;
	background: var(--MI_THEME-panel);
	color: var(--MI_THEME-fg);
	border-radius: var(--MI-radius);
}

.content h1 {
	margin-top: 0;
}

.notice {
	padding: 16px;
	margin-bottom: 24px;
	background: var(--MI_THEME-infoWarnBg);
	color: var(--MI_THEME-infoWarnFg);
	border: 1px solid var(--MI_THEME-warn);
	border-radius: var(--MI-radius);
}

.notice p {
	margin-bottom: 0;
}
</style>
