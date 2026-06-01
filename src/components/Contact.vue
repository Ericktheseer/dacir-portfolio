<script setup>

    import {Notyf} from 'notyf';

    import {ref, onMounted, onBeforeUnmount} from 'vue';
    
    const notyf = new Notyf();

    const name = ref("");
    const email = ref("");
    const message = ref("");
    const isLoading = ref(false);

    
    const WEB3FORMS_ACCESS_KEY = "8d9e0adf-ed1e-452c-8e09-42b6cc8fa7ed"; // Replace with your web3forms
    const subject = "New message from Portfolio Contact Form";
    const submitForm = async () => {

        
        if(!recaptchaToken.value) {
            notyf.error('Please verify that you are not a robot');
            
            return;
        }

        isLoading.value = true;

        try {
            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },                
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            })

            const result = await response.json();

                
                if (result.success) {
                    console.log(result);
                    isLoading.value = false;
                    notyf.success("Message Sent!");
                }
        } catch (error) {
            console.log(error);
            isLoading.value = false;
            notyf.error("Failed to send message.");
        } finally {
            resetRecaptcha();
        }

    }
    
    const SITE_KEY = '6Lc-cQctAAAAAAVL1M_f8Pc3ds-oe3_e1USAYXar';  // Replace with your site key

    
    const recaptchaContainer = ref(null);    
    const recaptchaWidgetId = ref(null);    
    const recaptchaToken = ref('');

    
    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }


    function renderRecaptcha() {
        if (!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired,
        });
    }

    
    function resetRecaptcha() {
        if (recaptchaWidgetId.value !== null) {
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
        }
    }

    onMounted(() => {
        
    const interval = setInterval(() => {
        if (window.grecaptcha && window.grecaptcha.render) {
            renderRecaptcha();            
            clearInterval(interval);
        }
        
    }, 100);

    
      onBeforeUnmount(() => {
          clearInterval(interval);
      });
    });  

</script>

<template>
	
	<!-- Start of Contact -->
            <section class="container-fluid bg-primary" id="contact">
                        
                <section class="row">
                    <div class="col-md-6 text-center d-flex flex-column justify-content-center align-items-center py-5 py-md-0" id="contact-image">
                        
                    </div>
                    <div class="col-md-6 p-5">
                        <!-- form -->               
                        <h2 class="pt-5 pb-3 text-center name">
                            Contact Us!
                        </h2>
                        
                        <form class="mt-3">
                          <div class="form-group my-4 rounded-3">
                            <label for="exampleFormControlInput1">Name</label>
                            <input type="text" class="form-control" id="formname" placeholder="First Name M.I. Last Name">
                          </div>
                          <div class="form-group my-4 rounded-3">
                            <label for="exampleFormControlInput1">Email Address</label>
                            <input type="email" class="form-control" id="formemail" placeholder="john.doe@email.com">
                          </div>
                          <div class="form-group my-4 rounded-3">
                            <label for="exampleFormControlTextarea1">Message</label>
                            <textarea class="form-control" id="formmessage" rows="6"></textarea>
                          </div>
                          <div class="my-3 d-flex justify-content-between">
                            <div class="d-flex justify-content-start gap-3">
                                <a href="https://www.linkedin.com/" id="linkedin"><i class="fab fa-linkedin"></i></a>
                                <a href="https://github.com/Ericktheseer/webportfolio" id="gitlab"><i class="fab fa-gitlab"></i></a>
                                <a href="https://github.com/" id="github"><i class="fab fa-github"></i></a>
                            </div>  

                            <button type="submit" id="form-btn" class="submit-btn pl-5 pr-5" :disabled="isLoading">{{isLoading ? "Sending..." : "Submit"}}<i class="bi bi-send-fill"></i></button>
                          </div>

                          <!-- The location where the reCAPTCHA checkbox will appear -->
                          <div class="d-flex justify-content-end mt-2">
                              <!-- Creates a reference to the div so the reCAPTCHA widget can be rendered into it. -->
                              <div ref="recaptchaContainer"></div>
                          </div>
                        </form>
                    </div>
                </section>

            </section>
            <!-- End of Contact -->
	
</template>

<style scoped>
	
</style>