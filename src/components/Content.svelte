<script>
    import {createEventDispatcher} from "svelte"
    import {keyItems} from "../store"
    import Icon from 'svelte-icons-pack/Icon.svelte'
    import AiOutlineClose from 'svelte-icons-pack/fa/FaTrashAlt'

    export let data
    export let key
    export let color
    
    const emit = createEventDispatcher()
    
    $: {
        keyItems.subscribe(e=>{
            console.log(e[key], 'value store')
            data = e[key] 
        })
        console.log(color)
    }

    function handleClick(item){
        emit('handleClick', {data: item.data})
    }
    function removeFromCard(item){
        console.log(item.data, "removed frommmmmmmmmmmmmmmmm data")
        emit("removeFromCard", {data: item.data})
    }
    

</script>
<div   class="content">
    {#each data as item}
        <div on:click={handleClick(item)}  style={` background-color: ${color}`} class="info">
            <h3>{item.title}</h3>
            <span>{item.description}</span>
            <span on:click={removeFromCard(item)} class="close">
                <Icon src={AiOutlineClose} color="rgba(0,0,0,.4)" size="11" className="custom-icon" title="Custom icon params" />
            </span>
        </div>
    {/each}
</div>

<style>
    .content{
        overflow-y: auto;
        height: calc(100% - 19%);
        margin-top: -1px;
        padding: 10px;
	}
    :global(
    .container .row:first-child  .col:nth-child(1) .content,
    .container .row:first-child  .col:nth-child(3) .content,
    .container .row:first-child  .col:nth-child(5) .content){
        height: calc(100% - 10%) !important;
    }
	.info{   
		text-indent: 0;
		border-radius: 3px;
		font-size: 12px;
        box-shadow: 0 1px 1px 0 rgb(0,0,0,.4);
        margin-bottom: 12px;
        text-align: center;
        padding: 0px 20px 8px 20px;
        height: fit-content;
        flex: 1 0 fit-content;
        align-items: flex-start;
        position: relative;
	}
    .info span {
        display: block;
    }
    .info .close{
        color: #000;
        font-size: 15px;
        position: absolute;
        top: 12%;
        right: 5%;
        transition: ease-in-out 0.7s;
        cursor: pointer !important;
    }
    .info .close svg{
        cursor: pointer !important;
    }
    .info .close:hover{
        transform: scale(1.5);
        cursor: pointer;
    }
    :global(.row:last-child .content){
        display: flex;
        flex-wrap: wrap;
        align-items: start;
        gap: 33px;
    }
    .content h3, .content span{
        text-align: left;
    }
    .content h3{
        font-weight: bold;
        padding-top: 9px;
    }
    
</style>