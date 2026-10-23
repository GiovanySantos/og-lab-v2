<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<div v-if="meta" :class="$style.root">
	<MkFeaturedPhotos :class="$style.bg"/>
	<div :class="$style.contents">
		<MkVisitorDashboard/>
	</div>
</div>
</template>

<script lang="ts" setup>
import MkFeaturedPhotos from '@/components/MkFeaturedPhotos.vue';
import MkVisitorDashboard from '@/components/MkVisitorDashboard.vue';
import { instance as meta } from '@/instance.js';
</script>

<style lang="scss" module>
.root {
	height: 100cqh;
	overflow: auto;
	overscroll-behavior: contain;
	background: var(--MI_THEME-bg);
	color: var(--MI_THEME-fg);
	--MI_THEME-bg: #24211D;
	--MI_THEME-panel: #302B25;
	--MI_THEME-panelHighlight: #383229;
	--MI_THEME-fg: #E7D7B8;
	--MI_THEME-fgHighlighted: #E7D7B8;
	--MI_THEME-fgOnPanel: #E7D7B8;
	--MI_THEME-accent: #D87536;
	--MI_THEME-fgOnAccent: #24211D;
	--MI_THEME-buttonGradateA: #D87536;
	--MI_THEME-buttonGradateB: #D87536;
	--MI_THEME-buttonBg: #3B342A;
	--MI_THEME-buttonHoverBg: #484031;
	--MI_THEME-divider: color-mix(in srgb, #E7D7B8 16%, transparent);
	--MI_THEME-focus: #B99558;
	--MI_THEME-link: #7DB8C9;
	--MI_THEME-success: #9BC28B;
	--MI_THEME-warn: #E3BB73;
	--MI_THEME-error: #F28B82;
	--MI_THEME-shadow: rgb(0 0 0 / 45%);
	--MI_THEME-header: color-mix(in srgb, #24211D 92%, transparent);
	--MI_THEME-windowHeader: color-mix(in srgb, #302B25 92%, transparent);
}

.bg {
	position: fixed;
	top: 0;
	right: 0;
	width: 100vw;
	height: 100vh;
	// 固定レイヤがホイール操作を奪い、コンテンツ列以外の上でページをスクロールできなくなるのを防ぐ (issue #17680)
	pointer-events: none;
}

.contents {
	position: relative;
	width: min(430px, calc(100% - 32px));
	margin: auto;
	padding: 100px 0 100px 0;
}
</style>
