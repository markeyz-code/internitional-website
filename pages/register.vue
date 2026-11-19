<template>
  <div class="min-h-screen bg-[#F6F8FB] lg:grid lg:grid-cols-[.92fr_1.08fr]">
    <!-- Brand panel -->
    <aside class="hidden lg:flex relative overflow-hidden bg-navy-950 p-10 xl:p-12 flex-col justify-between">
      <img src="https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?q=80&w=2000&auto=format&fit=crop" alt="Microscope" class="absolute inset-0 w-full h-full object-cover opacity-20" />
      <div class="absolute inset-0 navy-mesh"></div>
      <div class="absolute inset-0 brand-grid opacity-[0.08]"></div>
      <div class="relative z-10 flex items-center justify-between">
        <NuxtLink to="/" class="flex items-center gap-3">
          <BrandLogo variant="white" class="h-9" />
          <span class="font-display text-xl font-bold text-white">InternTional<span class="text-teal-400">.</span></span>
        </NuxtLink>
        <span class="rounded-full border border-white/15 bg-white/5 px-3 py-1.5 text-[10px] font-extrabold uppercase tracking-[0.16em] text-white/60">Professional application</span>
      </div>
      <div class="relative z-10 max-w-md">
        <p class="text-[11px] font-extrabold uppercase tracking-[0.2em] text-teal-300">Membership</p>
        <h2 class="mt-3 font-display text-4xl xl:text-5xl font-bold leading-[1.02] text-white">Join the verified<br>professional community.</h2>
        <p class="mt-4 text-sm leading-relaxed text-white/55">A focused clinical home for interns and practising scientists who take the bench seriously.</p>
        <div class="mt-8 space-y-3.5">
          <div class="flex items-center gap-3 text-sm text-white/70"><span class="w-8 h-8 rounded-xl bg-teal-400/10 border border-teal-400/20 flex items-center justify-center"><FileText class="w-4 h-4 text-teal-300" /></span> Posting letter, licence or degree</div>
          <div class="flex items-center gap-3 text-sm text-white/70"><span class="w-8 h-8 rounded-xl bg-brand-400/15 border border-brand-300/20 flex items-center justify-center"><ShieldCheck class="w-4 h-4 text-brand-200" /></span> Manual review in 24–48 hours</div>
          <div class="flex items-center gap-3 text-sm text-white/70"><span class="w-8 h-8 rounded-xl bg-teal-400/10 border border-teal-400/20 flex items-center justify-center"><Lock class="w-4 h-4 text-teal-300" /></span> No payment until you are approved</div>
        </div>
      </div>
      <div class="relative z-10 rounded-3xl border border-white/10 bg-white/[0.06] p-5 backdrop-blur">
        <p class="text-[10px] font-extrabold uppercase tracking-[0.16em] text-teal-300">Built for the bench</p>
        <p class="mt-1.5 text-sm leading-relaxed text-white/65">One current professional document. A human review. Then the workspace opens.</p>
      </div>
    </aside>

    <!-- Form panel -->
    <main class="relative flex justify-center p-5 sm:p-8 lg:p-10 overflow-y-auto min-h-screen">
      <div class="absolute inset-0 brand-grid opacity-50 lg:hidden"></div>
      <div class="relative w-full max-w-2xl my-auto py-4">
        <div class="mb-5 flex items-center justify-center lg:hidden">
          <NuxtLink to="/" class="flex items-center gap-2.5">
            <BrandLogo variant="brand" class="h-8" />
            <span class="font-display text-lg font-bold text-navy-950">InternTional<span class="text-teal-500">.</span></span>
          </NuxtLink>
        </div>

        <div class="rounded-[2rem] border border-navy-100 bg-white p-6 sm:p-8 shadow-soft">
        <div v-if="success" class="text-center py-6">
          <div class="w-16 h-16 bg-teal-50 text-teal-600 rounded-2xl flex items-center justify-center mx-auto mb-6">
            <CheckCircle2 class="w-8 h-8" />
          </div>
          <p class="text-[11px] font-extrabold uppercase tracking-[0.18em] text-brand">Application received</p>
          <h2 class="mt-2 font-display text-3xl font-bold text-navy-950">You’re in the review queue.</h2>
          <p class="mt-3 text-sm leading-relaxed text-navy-500 max-w-md mx-auto">Your document is pending manual review. If approved, you’ll receive an email; sign in then to choose your subscription plan. No payment is taken now.</p>
          <NuxtLink to="/" class="mt-7 inline-flex items-center justify-center rounded-full bg-navy-950 px-6 py-3 text-sm font-bold text-white hover:bg-navy-900 transition-colors">Return to homepage</NuxtLink>
        </div>

        <div v-else>
          <p class="text-[11px] font-extrabold uppercase tracking-[0.18em] text-brand">Professional membership</p>
          <h2 class="mt-2 font-display text-3xl font-bold text-navy-950">Apply for membership</h2>
          <p class="mt-1.5 text-sm text-navy-500">InternTional is exclusive to verified Medical Laboratory Science interns and professionals.</p>

          <div class="mb-7 mt-6 grid grid-cols-3 gap-2">
            <div v-for="(label, idx) in ['Details', 'Email', 'Verification']" :key="label">
              <div class="h-1.5 rounded-full transition-colors" :class="step >= idx + 1 ? 'bg-brand' : 'bg-navy-100'"></div>
              <p class="mt-2 text-[9px] font-extrabold uppercase tracking-[0.12em]" :class="step >= idx + 1 ? 'text-brand' : 'text-navy-300'">{{ label }}</p>
            </div>
          </div>

          <form @submit.prevent="submitStep" class="space-y-6">
            <div v-if="step === 1" class="space-y-6">
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                <UiInput id="firstName" label="First Name" v-model="form.firstName" required placeholder="Jane" />
                <UiInput id="lastName" label="Last Name" v-model="form.lastName" required placeholder="Doe" />
              </div>
              <UiInput id="email" label="Email Address" type="email" v-model="form.email" required placeholder="jane.doe@example.com" />
              <UiInput id="password" label="Password" type="password" v-model="form.password" required minlength="8" placeholder="Minimum 8 characters" />
            </div>

            <div v-else-if="step === 2" class="space-y-6">
              <div class="text-center">
                <div class="w-16 h-16 bg-blue-50 text-brand rounded-full flex items-center justify-center mx-auto mb-4">
                  <Mail class="w-8 h-8" />
                </div>
                <h3 class="text-xl font-medium text-navy-950 mb-2">Verify Your Email</h3>
                <p class="text-navy-400 mb-1 text-sm">We've sent a 4-digit code to <span class="font-bold">{{ form.email }}</span>.</p>
                <button type="button" @click="step = 1" class="text-brand text-sm font-medium hover:underline">Change email address</button>
              </div>
              
              <div class="flex gap-4 justify-center my-8">
                <input 
                  v-for="(digit, idx) in 4" 
                  :key="idx" 
                  ref="otpRefs" 
                  type="text" 
                  maxlength="1" 
                  v-model="otpArray[idx]" 
                  @input="handleOtpInput(idx, $event)" 
                  @keydown="handleOtpKeydown(idx, $event)" 
                  class="w-14 h-14 text-center text-2xl font-bold border-2 border-navy-100 rounded-2xl text-navy-950 focus:border-brand focus:ring-4 focus:ring-brand/10 outline-none transition-all" 
                />
              </div>

              <div class="text-center">
                <p v-if="countdown > 0" class="text-sm text-navy-400 mb-2">Code expires in <span class="font-bold text-navy-950">{{ formattedCountdown }}</span></p>
                <p v-else class="text-sm text-red-600 mb-2">Code expired!</p>
                
                <button type="button" @click="resendCode" :disabled="countdown > 540 || loading" class="text-brand text-sm font-medium hover:underline disabled:opacity-50 disabled:cursor-not-allowed">
                  {{ countdown > 540 ? `Resend code in ${countdown - 540}s` : 'Resend code' }}
                </button>
              </div>
            </div>

            <div v-else-if="step === 3" class="space-y-6">
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                <UiSelectSearch
                  id="country"
                  label="Country of Residence"
                  v-model="selectedCountryCode"
                  :options="countries"
                  :required="step === 3"
                  placeholder="Select a country"
                />
                <UiInput 
                  id="phoneNumber" 
                  label="Phone Number" 
                  type="tel" 
                  :modelValue="form.phoneNumber" 
                  @update:modelValue="onPhoneInput"
                  :required="step === 3" 
                  placeholder="+44 7700 900077" 
                />
              </div>
              
              <div>
                <UiSelectSearch
                  id="professionalBackground"
                  label="Professional Background / Department"
                  v-model="form.professionalBackground"
                  :options="departments"
                  :required="step === 3"
                  placeholder="Select Department"
                />
              </div>

              <div>
                <label class="block text-[11px] font-extrabold uppercase tracking-[0.1em] text-navy-600 mb-1">Type of Verification Document <span class="text-brand">*</span></label>
                <UiSelectSearch
                  id="verificationDocumentType"
                  v-model="form.verificationDocumentType"
                  :required="step === 3"
                  placeholder="Select the document you are uploading"
                  :options="documentTypes"
                />
                <p class="text-xs text-navy-500 mt-2 leading-relaxed">
                  Accepted: <strong>internship posting letter</strong>, <strong>provisional licence</strong>, <strong>degree/attestation</strong>, or <strong>hospital/lab ID</strong>. The document must show your name and current placement.
                </p>
              </div>

              <div class="pt-2">
                <UiFileInput
                  v-model="form.file"
                  label="Verification Document"
                  :required="step === 3"
                  accept="image/*,.pdf"
                  placeholder="Upload posting letter, licence, degree or hospital ID"
                  hint="PDF, JPG, PNG (max 5MB) · current placement"
                  :icon="UploadCloud"
                  :successIcon="CheckCircle2"
                />
                
                <div v-if="uploadProgress > 0 && uploadProgress < 100" class="mt-3">
                  <div class="flex justify-between text-xs text-navy-500 mb-1">
                    <span>Uploading...</span>
                    <span>{{ uploadProgress }}%</span>
                  </div>
                  <div class="w-full bg-navy-100 h-1.5 rounded-full overflow-hidden">
                    <div class="bg-brand h-full rounded-full transition-all" :style="{ width: uploadProgress + '%' }"></div>
                  </div>
                </div>

                <div class="mt-3 rounded-lg border border-amber-200 bg-amber-50 p-3 text-xs text-amber-900 leading-relaxed">
                  <strong>No payment yet.</strong> The technical team reviews your document manually. If approved, you will receive an email and can then sign in to choose a subscription plan.
                </div>
              </div>
            </div>

            <div v-if="error" class="text-red-700 text-sm p-4 bg-red-50 border border-red-200 flex flex-col gap-2 rounded">
              <div class="flex items-start gap-3">
                <Lock class="w-5 h-5 flex-shrink-0 mt-0.5 text-red-600" />
                <span class="font-medium">{{ error }}</span>
              </div>
              <div class="text-xs text-navy-500 pl-8">
                Need immediate help? Contact our support team at
                <a href="mailto:support@medlabconvo.com" class="text-brand font-semibold hover:underline">support@medlabconvo.com</a>
                or WhatsApp <strong class="text-navy-950">+234 813 000 0000</strong>.
              </div>
            </div>

            <div class="flex items-center gap-4 mt-4">
              <button 
                v-if="step === 3" 
                type="button" 
                @click="step = 2" 
                class="w-1/3 py-3 text-sm font-bold text-navy-600 bg-navy-50 hover:bg-navy-100 rounded-full transition-colors"
              >
                Back
              </button>
              <UiButton
                type="submit"
                :loading="loading"
                class="flex-1 py-3 text-base font-medium"
              >
                {{ step === 3 ? 'Submit Application' : (step === 2 ? 'Verify Email' : 'Next Step') }}
              </UiButton>
            </div>
          </form>

          <div class="mt-7 text-center pt-5 border-t border-navy-100">
            <p class="text-navy-500 text-sm">
              Already have an account?
              <NuxtLink to="/login" class="font-medium text-brand hover:underline ml-1">
                Sign in
              </NuxtLink>
            </p>
          </div>
        </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { useSeoMeta } from '#imports';

useSeoMeta({
  title: 'Apply for Membership - InternTional',
  description: 'Join the exclusive community of verified Medical Laboratory Science interns.',
  ogTitle: 'Apply for Membership - InternTional',
  ogDescription: 'Join the exclusive community of verified Medical Laboratory Science interns.',
  ogImage: 'https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?q=80&w=2000&auto=format&fit=crop',
  twitterCard: 'summary_large_image',
})

import { ref, watch, onMounted, onUnmounted, computed } from 'vue';
import { ArrowRight, UploadCloud, CheckCircle2, Lock, Mail, FileText, ShieldCheck } from 'lucide-vue-next';
import { useRegister } from '@/composables/modules/auth/useRegister';
import UiInput from '@/components/ui/Input.vue';
import UiButton from '@/components/ui/Button.vue';
import UiFileInput from '@/components/ui/FileInput.vue';
import UiSelectSearch from '@/components/ui/SelectSearch.vue';
import { getCountries, getCountryCallingCode, AsYouType, isValidPhoneNumber } from 'libphonenumber-js/min';
import { GATEWAY_ENDPOINT } from '@/api_factory/axios.config';

definePageMeta({ layout: 'empty' });

// Setup Countries List
let regionNames: Intl.DisplayNames | null = null;
try {
  regionNames = new Intl.DisplayNames(['en'], { type: 'region' });
} catch (e) {
  // fallback for unsupported environments
}

const countries = getCountries().map(code => {
  const name = regionNames ? regionNames.of(code) : code;
  return {
    label: `${name} (+${getCountryCallingCode(code)})`,
    value: code,
    name: name,
    callingCode: getCountryCallingCode(code),
  };
}).sort((a, b) => (a.name || '').localeCompare(b.name || ''));

const { loading, uploadProgress, error, register, sendOtp, verifyOtp } = useRegister();
const form = ref({ firstName: '', lastName: '', email: '', password: '', otp: '', country: '', phoneNumber: '', professionalBackground: '', verificationDocumentType: '', file: null as File | null });
const success = ref(false);
const step = ref(1);

const otpArray = ref(['', '', '', '']);
const otpRefs = ref<HTMLInputElement[]>([]);
const countdown = ref(0);
let countdownInterval: any = null;

const formattedCountdown = computed(() => {
  const m = Math.floor(countdown.value / 60).toString().padStart(2, '0');
  const s = (countdown.value % 60).toString().padStart(2, '0');
  return `${m}:${s}`;
});

const startCountdown = () => {
  countdown.value = 600;
  clearInterval(countdownInterval);
  countdownInterval = setInterval(() => {
    if (countdown.value > 0) {
      countdown.value--;
    } else {
      clearInterval(countdownInterval);
    }
  }, 1000);
};

const handleOtpInput = (idx: number, e: Event) => {
  const val = (e.target as HTMLInputElement).value;
  if (val && idx < 3) {
    otpRefs.value[idx + 1]?.focus();
  }
  form.value.otp = otpArray.value.join('');
};

const handleOtpKeydown = (idx: number, e: KeyboardEvent) => {
  if (e.key === 'Backspace' && !otpArray.value[idx] && idx > 0) {
    otpRefs.value[idx - 1]?.focus();
  }
};

const resendCode = async () => {
  const sent = await sendOtp(form.value.email, form.value.firstName, 'intern');
  if (sent) {
    startCountdown();
  }
};

onUnmounted(() => clearInterval(countdownInterval));

const selectedCountryCode = ref('');

const DEFAULT_DEPARTMENTS = [
  { label: 'Chemical Pathology / Clinical Chemistry', value: 'CHEMICAL_PATHOLOGY' },
  { label: 'Hematology & Blood Transfusion', value: 'HEMATOLOGY' },
  { label: 'Medical Microbiology & Parasitology', value: 'MICROBIOLOGY' },
  { label: 'Histopathology / Morbid Anatomy', value: 'HISTOPATHOLOGY' },
  { label: 'Medical Virology & Immunology', value: 'MEDICAL_VIROLOGY' },
  { label: 'General Clinical Laboratory Practice', value: 'GENERAL' },
];

const documentTypes = [
  { label: 'Internship posting letter', value: 'INTERNSHIP_POSTING_LETTER' },
  { label: 'Provisional / full licence', value: 'PROVISIONAL_LICENSE' },
  { label: 'Degree / attestation certificate', value: 'DEGREE_ATTESTATION' },
  { label: 'Hospital / laboratory ID', value: 'HOSPITAL_ID' },
  { label: 'Other professional document', value: 'OTHER_PROFESSIONAL' },
];

const departments = ref<{label: string; value: string}[]>(DEFAULT_DEPARTMENTS);

const fetchData = async () => {
  try {
    const deptRes = await GATEWAY_ENDPOINT.get('/departments');
    if (deptRes && Array.isArray(deptRes.data) && deptRes.data.length > 0) {
      departments.value = deptRes.data;
    }
  } catch (err) {
    console.warn('Using default departments list:', err);
  }

};

onMounted(() => {
  fetchData();
});

watch(selectedCountryCode, (newCode) => {
  if (newCode) {
    const c = countries.find(x => x.value === newCode);
    if (c) form.value.country = c.name || newCode;
    // Auto format existing phone
    if (form.value.phoneNumber) {
      const formatter = new AsYouType(newCode);
      form.value.phoneNumber = formatter.input(form.value.phoneNumber);
    }
  } else {
    form.value.country = '';
  }
});

const onPhoneInput = (val: string) => {
  if (selectedCountryCode.value) {
    const formatter = new AsYouType(selectedCountryCode.value);
    form.value.phoneNumber = formatter.input(val);
  } else {
    form.value.phoneNumber = val;
  }
};

const submitStep = async () => {
  error.value = null;

  if (step.value === 1) {
    if (!form.value.firstName || !form.value.lastName || !form.value.email || !form.value.password) {
      error.value = "Please fill in all basic details.";
      return;
    }
    const sent = await sendOtp(form.value.email, form.value.firstName, 'intern');
    if (sent) {
      step.value = 2;
      startCountdown();
    }
    return;
  }
  
  if (step.value === 2) {
    if (!form.value.otp || form.value.otp.length !== 4) {
      error.value = "Please enter the 4-digit code.";
      return;
    }
    const success = await verifyOtp(form.value.email, form.value.otp);
    if (success) step.value = 3;
    return;
  }

  if (!selectedCountryCode.value) {
    error.value = 'Please select your country of residence.';
    return;
  }
  if (!form.value.phoneNumber) {
    error.value = 'Please enter your phone number.';
    return;
  }
  if (!isValidPhoneNumber(form.value.phoneNumber, selectedCountryCode.value)) {
    error.value = 'Please enter a valid phone number for the selected country.';
    return;
  }
  if (!form.value.professionalBackground) {
    error.value = 'Please select a department.';
    return;
  }
  if (!form.value.verificationDocumentType) {
    error.value = 'Please select the type of document you uploaded.';
    return;
  }
  if (!form.value.file) {
    error.value = 'Please upload a verification document to proceed.';
    return;
  }

  const result = await register({
    firstName: form.value.firstName,
    lastName: form.value.lastName,
    email: form.value.email,
    password: form.value.password,
    country: form.value.country,
    phoneNumber: form.value.phoneNumber,
    professionalBackground: form.value.professionalBackground,
    verificationDocumentType: form.value.verificationDocumentType,
    file: form.value.file,
  });
  if (result) {
    if (result.authorization_url) {
      window.location.href = result.authorization_url;
    } else {
      success.value = true;
    }
  }
};
</script>
