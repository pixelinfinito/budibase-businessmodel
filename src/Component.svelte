<script>
  
  import { getContext } from "svelte"
  import Content from "./components/Content.svelte";
  import {keyItems} from "./store"
  import LinkIcon from "./icons/link-3-svgrepo-com.svg"
  import Chceck from "./icons/check-circle-svgrepo-com.svg"
  import Gift from "./icons/Gift.svg"
  import Heart from "./icons/heart-fill-svgrepo-com.svg"
  import Male from "./icons/male-face-icon.svg"
  import RourcesIcon from "./icons/manufacturing-process-engineer-icon.svg"
  import Truck from "./icons/logistic-truck-icon.svg"
  import Tags from "./icons/tags.svg"
  import MoneyBag from "./icons/money-bag-change-money-capital-svgrepo-com.svg"

  import lang from "./util/lang"

  export let language
  
  export let dataProvider
  export let mappingTitle
  export let mappingDescription
  export let cardColor
  let usageLanguage = lang['pt']
  
  export let clickEvent
  export let removeCard

  let data = []
  
  const keys = {
    keyPartners: [],
    keyResources: [],
    keyActivites: [],
    keyPropositions: [],
    keyRelationships: [],
    keySegments: [],
    keyStructure: [],
    keyStreams:[],
    keyChannels:[]
  }

  function clearCards (){
    if(dataProvider.rows){
      data = dataProvider.rows
      data.map(dataItem=>{
        console.log(keys, 'ite,cleaaaaaaaaaaaaaaaaaaaaaaaaaaaaaar', Object.keys(keys).length)   
        keys[dataItem.key] = []
      })
    }
  }

  $: {
    
    usageLanguage = lang[language ?? 'pt']
    if(dataProvider.rows){
      data = dataProvider.rows
      let counter = 0
      console.log(data, 'rows')

      
      clearCards()

      data.map(dataItem=>{
        
        console.log(keys, 'ite,mmmmmmmmmmmmmmmmmmmmmmmmmmmm', Object.keys(keys).length)

        if(counter == 0) keys[dataItem.key] = []
        keys[dataItem.key].push({
          data: dataItem,
          description: mappingDescription ? dataItem[mappingDescription] : dataItem.description,
          title: mappingTitle ? dataItem[mappingTitle] : dataItem.title
        })

        console.log('selected', keys)
        counter += 1
      })

      keyItems.set(keys)
    }
  }

  async function handleClick(data){
    await clickEvent({
      data: JSON.parse(JSON.stringify(data.detail))
    })
  }

  async function removeFromCard(eventData){
    console.log(eventData?.detail?.data, "compoentnet removed frommmmmmmmmmmmmmmmm data")
    await removeCard({
      data: JSON.parse(JSON.stringify(eventData?.detail?.data))
    })
  }

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

{#if dataProvider.rows.length > 0}
<div use:styleable={$component.styles}>
  <div  class="container">
		<div class="row">
      <div  class="col">
        <div class="head">
          <span >
            {@html LinkIcon}
          </span>
          <span> {usageLanguage.keyPartners }</span>
        </div>
        <Content color={cardColor ?? "#fdf092"} key={'keyPartners'} data={keys.keyPartners} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
      </div>
      <div  class="col">
        <div class="child-col">
          <div class="head">
            <span >
              {@html Chceck}
            </span>
            <span> {usageLanguage.keyActivites}</span>
          </div>
          <Content color={cardColor ?? "#fdf092"} key={'keyActivites'} data={keys.keyActivites} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
        </div>
        <div class="child-col">
          <div class="head">
            <span >
              {@html RourcesIcon}
            </span>
            <span>{usageLanguage.keyResources}</span>
          </div>
          <Content color={cardColor ?? "#fdf092"} key={'keyResources'} data={keys.keyResources} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
        </div>
      </div>
      <div class="col">
        <div class="head">
          <span>
            {@html Gift}
          </span>
          <span>{usageLanguage.keyPropositions}</span>
        </div>
        <Content  color={cardColor ?? "#fdf092"} key={'keyPropositions'} data={keys.keyPropositions} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
      </div>
      <div class="col">
        <div class="child-col">
          <div class="head">
            <span >
              {@html Heart}
            </span>
            <span>{usageLanguage.keyRelationships}</span>
          </div>
          <Content color={cardColor ?? "#fdf092"} key={'keyRelationships'} data={keys.keyRelationships} on:removeFromCard={(event)=> removeFromCard(event)}  on:handleClick={(event)=> handleClick(event)} />
        </div>
        <div class="child-col">
          <div class="head">
            <span >
              {@html Truck}
            </span>
            <span>{usageLanguage.keyChannels} </span>
          </div>
          <Content color={cardColor ?? "#fdf092"} key={'keyChannels'} data={keys.keyChannels} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
        </div>
      </div>
      <div class="col">
        <div class="head">
          <span >
            {@html Male}
          </span>
          <span>{usageLanguage.keySegments}</span>
        </div>
        <Content color={cardColor ?? "#fdf092"} key={'keySegments'} data={keys.keySegments} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
      </div>
		</div>
	
		<div class="row">
      <div class="col">
        <div class="head">
          <span >
            {@html Tags}
          </span>
          <span>{usageLanguage.keyStructure}</span>
        </div>
        <Content color={cardColor ?? "#fdf092"} key={'keyStructure'} data={keys.keyStructure} on:removeFromCard={(event)=> removeFromCard(event)}   on:handleClick={(event)=> handleClick(event)} />
      </div>
			<div class="col">
        <div class="head">
          <span >
            {@html MoneyBag}
          </span>
          <span>{usageLanguage.keyStreams}</span>
        </div>
        <Content color={cardColor ?? "#fdf092"} key={'keyStreams'} data={keys.keyStreams}  on:removeFromCard={(event)=> removeFromCard(event)} on:handleClick={(event)=> handleClick(event)} />
			</div>
		</div>
  </div>
</div>
{/if}

<style>
	.container{
		max-width: 100%;
		width: 100%;
		border: 1px solid #cdcfd2;
		max-height: 600px;
    border-radius: 1px;
		height: 600px;
		display: flex;
    background-color: #FFF;
		flex-direction: column;
    box-sizing: border-box;  
	}
	.container .row:last-child{
		border-top: 1px solid #cdcfd2;
		height: 30%;
		display: flex;
		justify-content: space-between;
	}
	.row{
		font-size: 0;
	}
	.container .row:last-child .col:first-child{
		border-right: 1px solid #cdcfd2;
	}
	.container .row:first-child{
		display: flex;
		height: 70%;
		justify-content: space-between;
	}
	.container .row:first-child  .col:nth-child(2), 
	.container .row:first-child  .col:nth-child(4){
		border-left: 1px solid #cdcfd2;
		border-right: 1px solid #cdcfd2;
	}
	:global(.container .row:first-child  .col, 
	.container .row:last-child .col){
		flex: 0 1 auto;
    overflow-wrap: break-word; 
	}
  
	.container .row:first-child  .col{
		max-width: calc(100%/5);
	}
  .container .row:last-child  .col{
		max-width: calc(100%/2);
	}
  .container .row:last-child  .col,
  .container .row:first-child  .col{
    width: 100%;
  }
  .container .row:first-child  .col .child-col:first-child{
    border-top: 0;
  }
	.col .child-col{
		height: 50%;
		border-top: 1px solid #cdcfd2;
	}
	.col span, .child-col span{
		text-align: left;
		font-weight: bold;
		font-size: 13px;
    color:#000;
	}
	.col, .child-col{
		text-indent: 9px;
	}
  .head{
    padding: 8px;
    display: flex;
    align-items: center;
  }
</style>