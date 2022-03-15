<style>
.tree, .tree ul, .tree li {
    list-style: none;
    margin: 0;
    padding: 0;
    position: relative;
}
.tree {
    margin: 0 0 1em;
    text-align: center;
}
.tree, .tree ul {
    display: table;
}
.tree ul {
    width: 100%;
}
.tree li {
    display: table-cell;
    padding: .5em 0;
    vertical-align: top;
}
.tree li:before {
    outline: solid 1px #666;
    content: "";
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
}
.tree li:first-child:before {
    left: 50%;
}
.tree li:last-child:before {
    right: 50%;
}
.tree span {
    border: solid .1em #666;
    border-radius: .3em;
    display: inline-block;
    margin: 0 .2em .5em;
    padding: .2em .5em;
    position: relative;
}
.tree ul:before,
.tree span:before {
    outline: solid 1px #666;
    content: "";
    height: .5em;
    left: 50%;
    position: absolute;
}
.tree ul:before {
    top: -.5em;
}
.tree span:before {
    top: -.55em;
}
.tree>li {
    margin-top: 0;
}
.tree>li:before,
.tree>li:after,
.tree>li>span:before {
    outline: none;
}
</style>

<ul class="tree">
  <li><span>John Smith</span>
    <ul>
      <li><span>2018</span>
        <ul>
          <li><span>Jan</span>
            <ul>
              <li><span>16th</span>
                <ul>
                  <li><span>4:00</span></li>
                </ul>
              </li>
            </ul>
          </li>
          <li> <span>Mar</span>
            <ul>
              <li><span>1st</span>
                <ul>
                  <li><span>12:00</span></li>
                  <li><span>14:00</span></li>
                </ul>
              </li>
              <li><span>2nd</span>
                <ul>
                  <li><span>12:00</span></li>
                  <li><span>14:00</span></li>
                </ul>
              </li>
              <li><span>3rd</span></li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </li>
</ul>