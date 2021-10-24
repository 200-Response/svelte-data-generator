<script>
    import axios from 'axios';
    import { toast } from '@zerodevx/svelte-toast';

    let  totalTpv, totalMessages,statusCode=["00","01","03","04","05","06","12","13","14","19","30","40","41","54","55","74","75","82","83","94"];
    
    async function generarStream (){
        let json = JSON.stringify({totalTpv,totalMessages,statusCode})
        await axios.post('http://ec2-54-234-93-168.compute-1.amazonaws.com:3000/message', json,{
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

    function addNumber(code) {
        if(statusCode.includes(code)){
            statusCode = [...statusCode.filter(m => m !== code)]
        }else{
            statusCode = [...statusCode, code]
        }
	}
    
    //:console.log(statusCode)

    let tpvStatus =[
    {        
        statusCode: "00",
        description: "Aprobada",
        notify: "success"
    },
    {
        statusCode: "01",
        description: "Referida",
        notify: "warning"
    },
    {     
        statusCode: "03",
        description: "Negocio inválido",
        notify: "warning"
    },
    {     
        statusCode: "04",
        description: "Recoger tarjeta",
        notify: "error"
    },
    {     
        statusCode: "05",
        description: "Rechazada",
        notify: "error"
    },
    {     
        statusCode: "06",
        description: "Reintente",
        notify: "error"
    },
    {     
        statusCode: "12",
        description: "Transacción no permitida",
        notify: "error"
    },
    {     
        statusCode: "13",
        description: "Monto inválido",
        notify: "error"
    },
    {     
        statusCode: "14",
        description: "Tarjeta inválida",
        notify: "error"
    },
    {     
        statusCode: "19",
        description: "Reintente",
        notify: "warning"
    },
    {     
        statusCode: "30",
        description: "Error de Formato",
        notify: "warning"
    },
    {     
        statusCode: "40",
        description: "Función no soportada",
        notify: "warning"
    },
    {     
        statusCode: "41",
        description: "Recoger tarjeta",
        notify: "error"
    },
    {     
        statusCode: "54",
        description: "Tarjeta expirada",
        notify: "warning"
    },
    {     
        statusCode: "55",
        description: "NIP incorrecto",
        notify: "warning"
    },
    {     
        statusCode: "74",
        description: "Terminal NO Registrada",
        notify: "error"
    },
    {     
        statusCode: "75",
        description: "Número de intentos de NIP excedidos",
        notify: "error"
    },
    {     
        statusCode: "82",
        description: "CVV incorrecto",
        notify: "error"
    },
    {     
        statusCode: "83",
        description: "Rechazada",
        notify: "error"
    },
    {     
        statusCode: "94",
        description: "Declinada",
        notify: "error"
    }
]
</script>



  

<div class="bg-white py-6 sm:py-8 lg:py-12">
    <div class="max-w-screen-3xl px-4 md:px-8 mx-auto">
      <h2 class="text-gray-800 text-2xl lg:text-3xl font-bold text-center mb-4 md:mb-8">TVP Generador de datos</h2>
  
      <form on:submit|preventDefault={generarStream} class="w-full border rounded-lg mx-auto">
        <div class="grid gap-4 grid-cols-2 gap-4 p-4 md:p-8">
          <div class="form-control">
            <label for="tpv" class="text text-primary">Total de TPV's</label>
            <input bind:value={totalTpv} id='tpv' name="tpv" type="number" class="input input-bordered" />
          </div>
  
          <div class="form-control">
            <label for="mensajes" class="text text-primary">Total de Mensajes</label>
            <input bind:value={totalMessages}  id="mensajes" name="mensajes" type="number" class="input input-bordered" />
          </div>
        </div>

          <div class="grid gap-4 grid-cols-3 gap-4 p-4 md:p-8">
          {#each tpvStatus as tpvStatu }
          
            {#if (tpvStatu.notify == 'success')}
            <div class="p-6 card bordered alert-{tpvStatu.notify}">
                <div class="form-control">
                        <label class="cursor-pointer label  m-4">
                            <span class="label-text">{tpvStatu.description}</span> 
                            <input type="checkbox" checked="checked" class="toggle toggle-sm" 
                            bind:value={tpvStatu.statusCode} 
                            on:click="{addNumber(tpvStatu.statusCode)}" >
                        </label>
                    </div>
                </div>
            {/if}
            {#if (tpvStatu.notify == 'warning')}
                <div class="p-6 card bordered alert-{tpvStatu.notify}">
                    <div class="form-control">
                            <label class="cursor-pointer label  m-4">
                                <span class="label-text">{tpvStatu.description}</span> 
                                <input type="checkbox" checked="checked" class="toggle toggle-sm" 
                                bind:value={tpvStatu.statusCode} 
                                on:click="{addNumber(tpvStatu.statusCode)}" >
                            </label>
                        </div>
                    </div>
                    {/if}
                    {#if (tpvStatu.notify == 'error')}
                    <div class="p-6 card bordered alert-{tpvStatu.notify}">
                        <div class="form-control">
                                <label class="cursor-pointer label  m-4">
                                    <span class="label-text">{tpvStatu.description}</span> 
                                    <input type="checkbox" checked="checked" class="toggle toggle-sm" 
                                    bind:value={tpvStatu.statusCode} 
                                    on:click="{addNumber(tpvStatu.statusCode)}" >
                                </label>
                            </div>
                        </div>
                    {/if}
            {/each}
        
        </div>
        <div class="form-control grid gap-4 grid-cols-1 gap-4 p-4 md:p-8">

            <button class="btn btn-primary">Generar stream</button>
        </div>
      </form>
    </div>
  </div>
