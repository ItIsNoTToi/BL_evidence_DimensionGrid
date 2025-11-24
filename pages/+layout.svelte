<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';	
	globalThis.checkToday = (key, table, table_uutien) => {
		if(key){
			return `
				SELECT *, 1 AS priority FROM ${table_uutien}
				UNION ALL
				SELECT *, 0 AS priority FROM ${table}`
		}
		return `
				SELECT *, 1 AS priority FROM ${table}
				UNION ALL
				SELECT *, 0 AS priority FROM ${table_uutien}`
	}
	export let data;
</script>

<EvidenceDefaultLayout {data} hideBreadcrumbs=true hideHeader=true hideSidebar=true hideTOC=true fullWidth=true builtWithEvidence=false neverShowQueries=true>
	<slot slot="content" />
</EvidenceDefaultLayout>