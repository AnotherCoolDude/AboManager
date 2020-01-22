<script>
import Muuri from 'muuri';
import { onMount } from 'svelte';

let sections = 2;
let sectionElements = [];
let muuriGrids = [];
let filterboard;

let filter = [
    {title: 'expenses', el: ''},
    {title: 'earnings', el: ''}, 
    {title: 'household', el: ''},
    {title: 'entertainment', el: ''}
];

onMount(() => {
    sectionElements = [].slice.call(document.querySelectorAll(".filtersection-content"));
    
    sectionElements.forEach((el) => {  
        let muuriElement = new Muuri(el, {
            dragEnabled: true,
            items: '.filteritem',
            dragSort: () => {
              return muuriGrids;
            },
            layout: {
              alignRight: true
            },
            dragSortInterval: 0,
            dragReleaseDuration: 400,
            dragReleaseEasing: 'ease'
        });

        muuriElement.on('dragStart', (item) => {
            item.getElement().style.width = item.getWidth() + 'px';
            item.getElement().style.height = item.getHeight() + 'px';
        });
        
        muuriElement.on('dragReleaseEnd', (item) => {
            item.getElement().style.width = '';
            item.getElement().style.height = '';
            muuriGrids.forEach((muuri) => {
                muuri.refreshItems().layout();
            });
        });

        muuriElement.on('layoutStart', (items) => {
            console.log(items);
            filterboard.refreshItems().layout();
        })

        console.log(muuriElement);
        muuriGrids = [...muuriGrids, muuriElement];
    });

    filterboard = new Muuri(".filterboard", {
        layoutEasing: 'ease',
        dragEnabled: false,
        dragSortInterval: 0,
        dragStartPredicate: {
            handle: '.filtersection'
        },
        dragReleaseDuration: 400,
        dragReleaseEasing: 'ease'
    });

    setTimeout(() => {
      muuriGrids.forEach(g => {
        g.refreshItems().layout(true);
      })
    })
})
</script>

<style>

.filterboard {
    position: relative;
    min-height: 500px;
}

.filtersection {
  position: absolute;
  left: 0;
  right: 0;
  margin: 0 1.5%;
  background: #f0f0f0;
  border-radius: 3px;
  z-index: 1;
}

.filtersection.muuri-item-releasing {
  z-index: 2;
}

.filtersection.muuri-item-dragging {
  z-index: 3;
  cursor: move;
}

.filtersection-handle {

}

.filtersection-content {
  position: relative;
  border: 10px solid transparent;
  min-height: 95px;
  width: 100%;
}

.filteritem {
  display: block;
  position: absolute;
  margin: 5px;
}

.filteritem.muuri-item-releasing {
  z-index: 9998;
}
.filteritem.muuri-item-dragging {
  z-index: 9999;
  cursor: move;
}
.filteritem.muuri-item-hidden {
  z-index: 0;
}

.filteritem-content {
  position: relative;
  cursor: pointer;
  -webkit-box-shadow: 0px 1px 3px 0 rgba(0,0,0,0.2);
  box-shadow: 0px 1px 3px 0 rgba(0,0,0,0.2);
}
</style>


<div class="filterboard">
    {#each Array(sections) as _, i}
    <div class="filtersection">
        <div class="filtersection-content" bind:this={sectionElements[i]}>
            {#each filter as f}
            <div class="filteritem">
                <div class="filteritem-content tag">
                    {f.title}
                </div>
            </div>
            {/each}
        </div>
        <div class="filtersection-handle">

        </div>
    </div>
    {/each}
</div>




