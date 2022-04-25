<script>
import gotham from '../../../data/gotham.csv'

function diff(obj1, obj2) {
    var result = {};
    for (var i in obj1) {
        if (!obj2.hasOwnProperty(i)) continue;
        if (obj1[i] !== obj2[i])
            result[i] = obj2[i];
    }
    return result;
}


</script>
