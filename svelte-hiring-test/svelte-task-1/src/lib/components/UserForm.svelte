<script lang="ts">
    // TODO: Implement form state management
    // TODO: Implement form validation
    // TODO: Implement submit handler
    // TODO: Implement success state management
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let data: App.UserFormData = {
        firstName: '',
        lastName: '',
        email: '',
        password: ''
    };
    let submittedData: App.UserFormData = {
        firstName: '',
        lastName: '',
        email: ''
    };
    let formErrors: App.FormErrors = {};
    let success = false;
    const validateEmail = (email: string) => {
        return email.includes('@') && email.includes('.');
    }
    const validatePassword = (password: string) => {
        return password.length >= 6;
    }
    const handleSubmit = () => {
        formErrors = {};
        success = false;
        if(!validateEmail(data.email)) formErrors.email = 'Invalid email';
        if(!validatePassword(data.password)) formErrors.password = 'Password must be at least 6 characters';
        if(Object.keys(formErrors).length === 0) {
            success = true;
        }
        //last name required
        if(!data.lastName) formErrors.lastName = 'Last name is required';
        if(!data.firstName) formErrors.firstName = 'First name is required';
        if(Object.keys(formErrors).length === 0) {
            success = true;
            
            //clear form errors
            formErrors = {};
            //hide the success message
            success = true;
            //displaying submitted data below the form excluding the password
            submittedData.firstName =  data.firstName;
            submittedData.lastName = data.lastName;
            submittedData.email = data.email;
            //clear form
            data = {
                firstName: '',
                lastName: '',
                email: '',
                password: ''
            };

            //remove the .hidden from .submitted-data
            document.querySelector('.submitted-data')?.classList.remove('hidden');  
        }
    }
    //hide the submitted data when the form is being filled again
    $: if(data.firstName || data.lastName || data.email || data.password) {
        success = false;
        //remove the .hidden from .submitted-data
        document.querySelector('.submitted-data')?.classList.add('hidden');
    }
</script>

<div class="form-container">
    <form on:submit|preventDefault={handleSubmit}>
        <div class="form-group">
            <label for="firstName">First Name</label>
            <input
                bind:value={data.firstName}
                id="firstName"
                type="text"
                placeholder="Enter your first name"
            />
            {#if formErrors.firstName}
                <span class="error-message">{formErrors.firstName}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="lastName">Last Name</label>
            <input
                bind:value={data.lastName}
                id="lastName"
                type="text"
                placeholder="Enter your last name"
            />
            {#if formErrors.lastName}
                <span class="error-message">{formErrors.lastName}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="email">Email</label>
            <input
                bind:value={data.email}
                id="email"
                type="email"
                placeholder="Enter your email"
            />
            {#if formErrors.email}
                <span class="error-message">{formErrors.email}</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="password">Password</label>
            <input
                bind:value={data.password}
                id="password"
                type="password"
                placeholder="Enter your password"
            />
            {#if formErrors.password}
                <span class="error-message">{formErrors.password}</span>
            {/if}
        </div>

        <button type="submit" class="submit-button">Submit</button>
    </form>


    <!-- TODO: Add success message section here -->
    {#if success}
        <span class="success-message">Form submitted successfully!</span>
    {/if}

    <!-- TODO: Add submitted data section here when formSubmitted dispatched -->
    <div class="submitted-data hidden">
        <h2>Submitted Data</h2>
        <p>First Name: {submittedData.firstName}</p>
        <p>Last Name: {submittedData.lastName}</p>
        <p>Email: {submittedData.email}</p>
    </div>
</div>

<style>
    .hidden {
        display: none;
    }
    .form-container {
        max-width: 480px;
        margin: 0 auto;
        padding: 2rem;
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .form-group {
        margin-bottom: 1.5rem;
    }

    label {
        display: block;
        margin-bottom: 0.5rem;
        font-weight: 500;
        color: #374151;
    }

    input {
        width: 100%;
        padding: 0.75rem;
        border: 1px solid #D1D5DB;
        border-radius: 6px;
        font-size: 1rem;
        transition: border-color 0.15s ease-in-out;
    }

    input:focus {
        outline: none;
        border-color: #3B82F6;
        box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
    }

    input::placeholder {
        color: #9CA3AF;
    }

    input.error {
        border-color: #EF4444;
    }

    .error-message {
        display: block;
        margin-top: 0.5rem;
        font-size: 0.875rem;
        color: #EF4444;
    }

    .submit-button {
        width: 100%;
        padding: 0.75rem 1.5rem;
        background-color: #3B82F6;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 1rem;
        font-weight: 500;
        cursor: pointer;
        transition: background-color 0.15s ease-in-out;
    }

    .submit-button:hover {
        background-color: #2563EB;
    }

    .submit-button:focus {
        outline: none;
        box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5);
    }

    .submit-button:disabled {
        background-color: #9CA3AF;
        cursor: not-allowed;
    }

    .success-message {
        margin-top: 1.5rem;
        padding: 1rem;
        background-color: #EDF7ED;
        border: 1px solid #C8E6C9;
        border-radius: 6px;
        color: #1B5E20;
    }
</style>