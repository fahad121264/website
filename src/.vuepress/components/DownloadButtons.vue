<template>
	<ul class="no-select download-list">
		<li>
			<a
				class="download-link"
				rel="noopener noreferrer"
				:href="
					browserDownloadUrl ||
						'https://github.com/inorichi/tachiyomi/releases/latest'
				"
				:title="tagName"
				download
			>
				<div class="download-button stable">
					<span class="download-area">
						<MaterialIcon
							name="download-get_app download-icons"
							icon-name="get_app"
						/>
						<span class="download-text-stable download-text"
							>Stable release</span
						>
					</span>
				</div>
			</a>
		</li>
		<li>
			<a class="download-link" href="http://tachiyomi.kanade.eu/latest">
				<div class="download-button dev">
					<div class="download-area">
						<MaterialIcon
							name="download-bug_report download-icons"
							icon-name="bug_report"
						/>
						<span class="download-text-dev download-text"
							>Dev release</span
						>
					</div>
				</div>
			</a>
		</li>
	</ul>
</template>

<style scoped lang="scss">
* {
	font-family: "Open Sans", Roboto, -apple-system, BlinkMacSystemFont,
		"Segoe UI", Oxygen, Ubuntu, Cantarell, "Fira Sans", "Droid Sans",
		"Helvetica Neue", sans-serif;
}

ul {
	width: 100%;
	text-align: center;
	margin: 0.625em 0 0 0;
	padding: 0;
}

li {
	display: inline-block;
	margin: 0.3125em 0.625em 0.3125em 0.625em;
}

a.download-link {
	&:hover {
		text-decoration: none;
	}
	.download-button {
		display: flex;
		cursor: pointer;
		justify-content: center;
		align-items: center;
		background-color: #2e84bf;
		width: 11.25em;
		height: 3.75em;
		border-radius: 0.3125em;
		padding: 0 1em 0 1em;
		&.stable {
			background-color: #2e84bf;
			&:hover {
				filter: brightness(110%);
			}
		}
		&.dev {
			background-color: #476582;
			&:hover {
				filter: brightness(110%);
			}
		}
		.download-area {
			font-size: 1.125em;
			color: #ffffff;
			display: flex;
			align-items: center;
			.download-icons {
				color: #ffffff;
				max-width: 2em;
				font-size: 0.875em;
			}
			.download-get_app {
				margin-left: 0.188em;
				margin-right: 0.25em;
			}
			.download-bug_report {
				margin-right: 0.25em;
			}
			.download-text-stable {
				margin-right: 0.375em;
			}
			.download-text-dev {
				margin-right: 0.063em;
			}
		}
	}
}
</style>

<script>
import axios from "axios";

const RELEASE_URL =
	"https://api.github.com/repos/inorichi/tachiyomi/releases/latest";

export default {
	data() {
		return {
			tagName: "",
			browserDownloadUrl: ""
		};
	},

	async mounted() {
		const { data } = await axios.get(RELEASE_URL);
		// Maybe eventually some release has more than the apk in assets.
		const apkAsset = data.assets.find(a => a.name.includes(".apk"));
		// Set the values.
		this.$data.tagName = data.tag_name;
		this.$data.browserDownloadUrl = apkAsset.browser_download_url;
	}
};
</script>
