<template>
	<header
		class="w-full flex justify-between items-center border-b-2 border-solid border-b-gray-200 py-2 px-4"
	>
		<!-- <div class="container mx-auto "> -->
		<NuxtLink to="/">
			<div class="w-24 md:(w-28)">
				<!-- <img class="cursor-pointer max-w-full" src="/tiktik-logo.png" /> -->
				<img class="cursor-pointer" :src="Logo" alt="TikTik logo" />
			</div>
		</NuxtLink>
		<!-- <i class="bi bi-archive"></i> -->
		<div class="relative">
			<form
				class="absolute md:static top-10 -left-20 bg-white inline-flex items-center"
				@submit.prevent="handleSearch"
			>
				<input
					type="text"
					v-model.trim="searchQuery"
					class="bg-primary p-3 font-medium border-2 border-gray-100 rounded-lg focus:(border-accent/50 outline-transparent) md:(top-0)"
					placeholder="Search accounts and videos"
				/>
				<button
					aria-label="Search accounts and videos"
					class="right-6 border-l-2 border-gray-300 pl-4 text-2xl text-gray-400"
				>
					<span class="mdi mdi-magnify"></span>
				</button>
			</form>
		</div>

		<div>
			<!-- <GoogleLogin
				ref="googleLoginEl"
				:clientId="config.public.GOOGLE_API_CLIENT_ID"
			/> -->
			<!-- :callback="handleLogin" -->
			<!-- :error="userStore.handleError" -->
			<div v-if="showGoogleLogin">
				<ClientOnly>
					<GoogleLogin
						ref="googleLoginEl"
						:clientId="config.public.GOOGLE_API_CLIENT_ID"
					/>
				</ClientOnly>
			</div>
		</div>
		<!-- :error="userStore.handleError" -->
		<!-- :callback="handleLogin" -->
	</header>
</template>

<!-- <template v-if="userComputed">
				<div class="flex gap-5 items-center md:(gap-10)">
					<NuxtLink to="/upload">
						<button
							class="border-2 px-2 text-md font-semibold flex items-center gap-2 md:(px-4)"
						>
							<span class="mdi mdi-plus text-xl"></span>
							<span class="hidden md:(block)">Upload</span>
						</button>
					</NuxtLink>
					<template v-if="userStore.getUser.image">
						<NuxtLink class="block w-full h-full" to="/">
							<img
								:src="userStore.getUser.image"
								class="block w-full h-full object-cover rounded-full aspect-square w-10 h-10 cursor-pointer"
								:alt="`photo of ${userStore.getUser.userName}'s profile`"
							/>
						</NuxtLink>
					</template>
					<button class="px-2" type="button" @click="handleLogout">
						<span class="mdi mdi-logout text-red-400 text-2xl"></span>
					</button>
				</div>
			</template> -->

<!-- <template v-else>
				<div v-if="showGoogleLogin">
					<GoogleLogin
						ref="googleLoginEl"
						:clientId="config.public.GOOGLE_API_CLIENT_ID"
						:callback="handleLogin"
						:error="userStore.handleError"
					/>
				</div>
			</template> -->

<script setup lang="ts">
	import {
		useSanityClient,
		// clientSanity as sanityClient,
	} from '~/utils/sanityClient';
	// import {
	// 	GoogleLogin,
	// 	googleLogout,
	// 	decodeCredential,
	// } from 'vue3-google-login';
	// import  '*.png' from './index.d.ts';
	let GoogleLogin = null;
	let googleLogout = null;
	let decodeCredential = null;

	const showGoogleLogin = ref(false);
	const config = useRuntimeConfig();

	if (process.client) {
		import('vue3-google-login').then((data) => {
			// console.log('data: ', data);
			const {
				GoogleLogin: GoogleLoginLocal,
				googleLogout: googleLogoutLocal,
				decodeCredential: decodeCredentialLocal,
			} = data;
			// console.log('GoogleLoginLocal: ', GoogleLoginLocal);
			// console.log('googleLogoutLocal: ', googleLogoutLocal);
			// console.log('decodeCredentialLocal: ', decodeCredentialLocal);

			GoogleLogin = GoogleLoginLocal;
			googleLogout = googleLogoutLocal;
			decodeCredential = decodeCredentialLocal;

			showGoogleLogin.value = true;
			console.log('showGoogleLogin.value: ', showGoogleLogin.value);
		});
	}
	// console.log('GoogleLogin: ', GoogleLogin);

	const googleLoginEl = ref(null);
	onMounted(() => {
		// console.log('googleLoginEl: ', googleLoginEl);
		// console.log('googleLoginEl.value.$el: ', googleLoginEl.value.$el);
		/* nextTick(() => {
			console.log('googleLoginEl: ', googleLoginEl);
		}); */
		if (process.client) {
			// const gBtnWrapper = document
			// 	.querySelector('.api-loading')
			// 	.classList.remove('api-loading');
			console.log('googleLoginEl?.value: ', googleLoginEl?.value);
			googleLoginEl?.value?.$el.classList.remove('api-loading');
		}
	});

	watch(showGoogleLogin, () => {
		if (process.client) {
			console.log('GoogleLogin: ', GoogleLogin);
			console.log('googleLogout: ', googleLogout);
			console.log('decodeCredential: ', decodeCredential);
			// const gBtnWrapper = document
			// 	.querySelector('.api-loading')
			// 	.classList.remove('api-loading');
			console.log('googleLoginEl?.value: ', googleLoginEl?.value);
			googleLoginEl?.value?.$el.classList.remove('api-loading');
		}
	});

	const clientSanity = useSanityClient(config);

	/* async function handleLogin(googleResponse: IGoogleResponse) {
		if (process.client) {
			try {
				const user = await userStore.createOrGetUser(googleResponse);
				const rawUser = toRaw(user);
				// console.log('rawUser: ', rawUser);

				const { data, error } = await useFetch(`/api/auth`, {
					method: 'POST',
					body: {
						user: rawUser,
					},
				});
				// console.log('data: ', data);

				if (data.value.statusCode !== 200) {
					throw new Error(`ошибка`);
				}

				// userStore.setIsLoginTrue();
				setUserCookie(rawUser);
			} catch (error) {
				userStore.setIsLoginFalse();
				// userStore.$state.isLogin = false;
			}
		}
	} */

	/* async function handleLogout() {
		if (process.client) {
			await googleLogout();

			userStore.clearUser();
			clearUserCookie();
		}
	} */

	// function handleError(error: any) {}
	const searchQuery = ref('');

	const router = useRouter();
</script>

<style lang="scss" scoped></style>
