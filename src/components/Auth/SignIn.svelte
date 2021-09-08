<script>
	import {supabase} from "../../supabaseClient"
	import { user } from '../../sessionStore';

	let loading = false
	let email;
	let password;

	const handleLogin = async () => {
		try {
			loading = true
			const { error } = await supabase.auth.signIn({ email:email, password:password })
			if (error) throw error
		} catch (error) {
			alert(error.error_description || error.message)
		} finally {
			loading = false
		}
	}

	if(supabase.auth.user()) {
		location.replace('/')
	}
</script>

<form class="row flex flex-center" on:submit|preventDefault={handleLogin}>
	<div class="col-6 form-widget">
		<h1 class="header">Supabase + Svelte</h1>
		<p class="description">Sign in via magic link with your email below</p>
		<div>
			<input
				class="inputField"
				type="email"
				placeholder="Email"
				bind:value={email}
			/>

			<input
				class="inputField"
				type="password"
				placeholder="Password"
				bind:value={password}
			/>
		</div>
		<div>
			<input type="submit" class='button block' value={loading ? "Loading" : "Sign In"} disabled={loading} />
		</div>
		<h2>
			Want to <a href='/signup'>Sign Up?</a>
		</h2>
	</div>
</form>

<style>
	a {
			text-decoration: underline;
	}
</style>