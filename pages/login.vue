<template>
  <div class="h-screen w-full">
    <div class="pt-20 sm:pt-0 lg:p-8">
      <div class="mx-auto flex w-full flex-col justify-center space-y-6">
        <div class="flex flex-col space-y-2 text-center">
          <h1 class="text-2xl font-semibold tracking-tight">Sign in your account</h1>
          <p class="text-sm text-muted-foreground">Enter your email and password to proceed</p>
        </div>
        <form @submit="submit">
          <fieldset :disabled="isSubmitting">
            <UiVeeInput
              icon="lucide:user"
              type="email"
              name="email"
              label="Email"
              placeholder="sample@valid.com"
            />
            <UiVeeInput
              icon="lucide:lock"
              type="password"
              name="password"
              label="Password"
              placeholder="***********"
            />
            <div class="pt-4">
              <UiButton type="submit" size="lg" class="w-full">Log In</UiButton>
            </div>
          </fieldset>
          <div class="pt-4">
            <UiDivider label="or continue with" class="text-muted-foreground" />
            <UiButton @click="signInWithGoogle" type="button" class="w-full" variant="outline">
              <Icon name="logos:google-icon" />
              Sign in with Google
            </UiButton>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
  import { provider } from "~/composables/useGoogle";
  import { signInWithEmailAndPassword, signInWithPopup } from "firebase/auth";

  definePageMeta({
    layout: "no-nav",
    middleware: ["already-signed-in"],
  });

  const auth = useFirebaseAuth()!;
  const { handleSubmit, isSubmitting } = useForm({
    validationSchema: toTypedSchema(loginSchema),
  });

  const submit = handleSubmit(async (values) => {
    const loading = useSonner.loading("Signing in", {
      description: "Please wait while we sign you in",
    });

    try {
      await signInWithEmailAndPassword(auth, values.email, values.password);
      useSonner.success("Sign in successful", {
        id: loading,
      });
      return navigateTo("/", { replace: true });
    } catch (error: any) {
      useSonner.error(error.message, {
        id: loading,
      });
    }
  });

  const signInWithGoogle = async () => {
    const loading = useSonner.loading("Signing in", {
      description: "Please wait while we sign you in",
    });

    try {
      await signInWithPopup(auth, provider);
      useSonner.success("Sign in successful", {
        id: loading,
      });
      return navigateTo("/", { replace: true });
    } catch (error: any) {
      useSonner.error(error.message, {
        id: loading,
      });
    }
  };
</script>

<style></style>
