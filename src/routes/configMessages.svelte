<script>
    import axios from 'axios';
    import { toast } from '@zerodevx/svelte-toast';
    
	import { onMount } from 'svelte';

    let email

	onMount(async () => {
		const res = await fetch(`https://czq95uqrig.execute-api.us-east-1.amazonaws.com/production/email/list`);
		email = await res.json();
	});

    async function generarStream (){
        let json = JSON.stringify({email})
        await axios.post('https://czq95uqrig.execute-api.us-east-1.amazonaws.com/production/email/add', json,{
            headers: {'Content-Type': 'application/json'}
        })
        .then(response => {
            console.log('Response', response);
            toast.push('Datos Generados correctamente', {
            theme: {
                '--toastBackground': '#48BB78',
                '--toastProgressBackground': '#2F855A'
            }
            })
        })
        .catch(error => {
            toast.push('Ha ocurrido algo inesperado', {
            theme: {
                '--toastBackground': '#F56565',
                '--toastProgressBackground': '#C53030'
            }
            })
        });
    }
</script>


    <section class="text-gray-600 body-font relative">
        <div class="container px-5 py-24 mx-auto">
          <div class="flex flex-col text-center w-full mb-12">
            <h1 class="sm:text-3xl text-2xl font-medium title-font mb-4 text-gray-900">Lista de correo</h1>
          </div>
          <div class="lg:w-1/2 md:w-2/3 mx-auto">
            <div class="flex flex-wrap -m-2">
              <div class="p-2 w-full">
                <div class="relative">
                    <form on:submit|preventDefault={generarStream} class=" rounded-lg mx-auto">
                        <div class="text-center mb-12">
                            <div class="form-control">
                                <label for="email" class="text text-primary mb-6">Ingrese los correos separados por coma </label>
                                <label for="email" class="text text-accent mb-6">Ej: juan@gmail.com, pedro@aol.com </label>
                                <textarea bind:value={email} id='email' name="email" type="email" class="textarea h-24 textarea-bordered"></textarea>
                            </div>
                            <div class="form-control mt-6">
                                <button class="btn btn-primary">Guardar lista</button>
                            </div>
                        </div>
                      </form>
                </div>
              </div>
            </div>
        </div>
    </section>
