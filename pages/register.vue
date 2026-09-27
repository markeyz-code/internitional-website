<template>
  <div class="min-h-screen bg-white flex">
    <!-- Image Side -->
    <div class="hidden lg:block lg:w-1/2 relative bg-gray-100">
      <img src="https://images.unsplash.com/photo-1582719478250-c89cae4dc85b?q=80&w=2000&auto=format&fit=crop" alt="Microscope" class="absolute inset-0 w-full h-full object-cover" />
      <div class="absolute inset-0 bg-brand/90 flex flex-col justify-between p-12">
        <NuxtLink to="/">
          <img src="~/assets/logo.jpg" class="h-10 w-auto rounded-lg" alt="InternTional Logo" />
        </NuxtLink>
        <div class="text-white space-y-4 max-w-md">
          <h2 class="text-4xl font-medium leading-tight">Join the exclusive community.</h2>
          <p class="text-blue-100 font-light">Connect with mentors, access premium clinical guides, and elevate your MLS internship.</p>
        </div>
      </div>
    </div>

    <!-- Form Side -->
    <div class="w-full lg:w-1/2 flex justify-center p-8 sm:p-12 overflow-y-auto h-screen">
      <div class="w-full max-w-xl my-auto py-8">
        <div class="mb-10 lg:hidden text-center">
          <NuxtLink to="/">
            <img src="~/assets/logo-icon.png" class="h-8 w-auto mx-auto mb-2" alt="InternTional Logo" />
          </NuxtLink>
        </div>
        
        <div v-if="success" class="text-center">
          <div class="w-16 h-16 bg-green-50 text-green-600 rounded-full flex items-center justify-center mx-auto mb-6">
            <ArrowRight class="w-8 h-8" />
          </div>
          <h2 class="text-3xl font-medium text-gray-900 mb-4">Application Submitted</h2>
          <p class="text-gray-500 mb-8 max-w-md mx-auto">Your account is pending admin review. You'll receive an email notification once your verification document is approved.</p>
          <NuxtLink to="/" class="inline-block bg-gray-900 text-white px-6 py-2.5 rounded font-medium hover:bg-gray-800 transition-colors">
            Return to Homepage
          </NuxtLink>
        </div>

        <div v-else>
          <h2 class="text-3xl font-medium text-gray-900 mb-2">Apply for Membership</h2>
          <p class="text-gray-500 mb-8">InternTional is exclusive to verified MLS interns.</p>

          <!-- Step Indicators -->
          <div class="flex items-center mb-8 gap-2">
            <div class="flex-1 h-2 rounded-full transition-colors" :class="step >= 1 ? 'bg-brand' : 'bg-gray-200'"></div>
            <div class="flex-1 h-2 rounded-full transition-colors" :class="step >= 2 ? 'bg-brand' : 'bg-gray-200'"></div>
            <div class="flex-1 h-2 rounded-full transition-colors" :class="step >= 3 ? 'bg-brand' : 'bg-gray-200'"></div>
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

            <div v-if="step === 2" class="space-y-6">
              <div class="text-center">
                <div class="w-16 h-16 bg-blue-50 text-brand rounded-full flex items-center justify-center mx-auto mb-4">
                  <Mail class="w-8 h-8" />
                </div>
                <h3 class="text-xl font-medium text-gray-900 mb-2">Verify Your Email</h3>
                <p class="text-gray-500 mb-1 text-sm">We've sent a 4-digit code to <span class="font-bold">{{ form.email }}</span>.</p>
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
                  class="w-16 h-16 text-center text-3xl font-bold border-2 border-gray-200 rounded-xl text-gray-900 focus:border-brand focus:ring-4 focus:ring-brand/20 outline-none transition-all" 
                />
              </div>

              <div class="text-center">
                <p v-if="countdown > 0" class="text-sm text-gray-500 mb-2">Code expires in <span class="font-bold text-gray-900">{{ formattedCountdown }}</span></p>
                <p v-else class="text-sm text-red-600 mb-2">Code expired!</p>
                
                <button type="button" @click="resendCode" :disabled="countdown > 540 || loading" class="text-brand text-sm font-medium hover:underline disabled:opacity-50 disabled:cursor-not-allowed">
                  {{ countdown > 540 ? `Resend code in ${countdown - 540}s` : 'Resend code' }}
                </button>
              </div>
            </div>

            <div v-if="step === 3" class="space-y-6">
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                <UiSelectSearch
                  id="country"
                  label="Country of Residence"
                  v-model="selectedCountryCode"
                  :options="countries"
                  required
                  placeholder="Select a country"
                />
                <UiInput 
                  id="phoneNumber" 
                  label="Phone Number" 
                  type="tel" 
                  :modelValue="form.phoneNumber" 
                  @update:modelValue="onPhoneInput"
                  required 
                  placeholder="+44 7700 900077" 
                />
              </div>
              
              <div>
                <UiSelectSearch
                  id="professionalBackground"
                  label="Professional Background / Department"
                  v-model="form.professionalBackground"
                  :options="departments"
                  required
                  placeholder="Select Department"
                />
              </div>

              <div>
                <label class="block text-sm font-medium text-gray-700 mb-2">Select Subscription Plan</label>
                <div class="space-y-3">
                  <div 
                    v-for="plan in plans" 
                    :key="plan._id"
                    @click="form.planId = plan._id"
                    class="border rounded-lg p-3 cursor-pointer transition-all flex items-center justify-between"
                    :class="form.planId === plan._id ? 'border-brand bg-brand/5 ring-1 ring-brand' : 'border-gray-200 hover:border-brand/50'"
                  >
                    <div class="flex items-center gap-3">
                      <div class="w-4 h-4 rounded-full border flex-shrink-0 flex items-center justify-center mt-0.5" :class="form.planId === plan._id ? 'border-brand bg-brand' : 'border-gray-300'">
                        <div v-if="form.planId === plan._id" class="w-1.5 h-1.5 bg-white rounded-full"></div>
                      </div>
                      <div>
                        <h3 class="font-bold text-gray-900 text-sm">{{ plan.name }}</h3>
                        <p class="text-xs text-gray-500 line-clamp-1">{{ plan.description }}</p>
                      </div>
                    </div>
                    <div class="text-right flex-shrink-0 ml-4">
                      <div class="font-bold text-gray-900 text-sm">₦{{ (plan.price / 100).toLocaleString() }}</div>
                      <div class="text-[10px] text-gray-500 uppercase tracking-wider">/ {{ plan.durationMonths }} mo</div>
                    </div>
                  </div>
                </div>
                <p class="text-xs text-gray-500 mt-3 flex items-start gap-1.5 bg-gray-50 p-2 rounded">
                  <span class="text-brand font-bold shrink-0">ⓘ Note:</span>
                  <span>You will be securely redirected to Paystack to enter your card details. Your card will be automatically charged at the intervals defined by your chosen plan. You can cancel at any time.</span>
                </p>
              </div>

              <div class="pt-2">
                <UiFileInput
                  v-model="form.file"
                  label="Verification Document"
                  :required="step === 3"
                  accept="image/*,.pdf"
                  placeholder="Upload Posting Letter or Lab ID"
                  hint="PDF, JPG, PNG (max 5MB)"
                  :icon="UploadCloud"
                  :successIcon="CheckCircle2"
                />
                
                <div v-if="uploadProgress > 0 && uploadProgress < 100" class="mt-3">
                  <div class="flex justify-between text-xs text-gray-600 mb-1">
                    <span>Uploading...</span>
                    <span>{{ uploadProgress }}%</span>
                  </div>
                  <div class="w-full bg-gray-200 h-1.5 rounded-full overflow-hidden">
                    <div class="bg-brand h-full rounded-full transition-all" :style="{ width: uploadProgress + '%' }"></div>
                  </div>
                </div>
              </div>
            </div>

            <div v-if="error" class="text-red-700 text-sm p-4 bg-red-50 border border-red-200 flex items-start gap-3 rounded">
              <Lock class="w-5 h-5 flex-shrink-0 mt-0.5" />
              <span>{{ error }}</span>
            </div>

            <div class="flex items-center gap-4 mt-4">
              <button 
                v-if="step === 3" 
                type="button" 
                @click="step = 2" 
                class="w-1/3 py-3 text-base font-medium text-gray-700 bg-gray-100 hover:bg-gray-200 rounded"
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

          <div class="mt-8 text-center pt-6 border-t border-gray-100">
            <p class="text-gray-600 text-sm">
              Already have an account?
              <NuxtLink to="/login" class="font-medium text-brand hover:underline ml-1">
                Sign in
              </NuxtLink>
            </p>
          </div>
        </div>
      </div>
    </div>
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

import { ref, watch, onMounted } from 'vue';
import { ArrowRight, UploadCloud, CheckCircle2, Lock, Mail } from 'lucide-vue-next';
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
const form = ref({ firstName: '', lastName: '', email: '', password: '', otp: '', country: '', phoneNumber: '', professionalBackground: '', planId: '', file: null as File | null });
const success = ref(false);
const step = ref(1);

const selectedCountryCode = ref('');
const departments = ref<{label: string; value: string}[]>([]);
const plans = ref<any[]>([]);

const fetchData = async () => {
  try {
    const [deptRes, plansRes] = await Promise.all([
      GATEWAY_ENDPOINT.get('/departments'),
      GATEWAY_ENDPOINT.get('/subscriptions')
    ]);
    departments.value = deptRes.data || [];
    plans.value = plansRes.data || plansRes;
  } catch (err) {
    console.error('Failed to fetch data:', err);
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
    const success = await sendOtp(form.value.email, form.value.firstName, 'intern');
    if (success) {
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
  if (!form.value.planId) {
    error.value = 'Please select a subscription plan.';
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
    planId: form.value.planId,
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
