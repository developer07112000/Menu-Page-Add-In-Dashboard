# Add Menu-Page-Add-In-Dashboard

=============================In function.php file ==================================================
 add_action( 'admin_menu', 'wpse_91693_register' );
function wpse_91693_register()
{
    add_menu_page(
        'User Details',   
        'Users Details',  
        'manage_options',  
        'new_details',     
        'wpse_91693_render', 
        'dashicons-chart-pie',
        5 
    );
}
function wpse_91693_render()
{
    global $title;
    global $wpdb;
    $items_per_page = 10;
    $page = isset( $_GET['cpage'] ) ? abs( (int) $_GET['cpage'] ) : 1;
    $offset = ( $page * $items_per_page ) - $items_per_page;
    $qa_table = $wpdb->prefix.'membership_userdata';
    $total_query = "SELECT COUNT(*) FROM $your_table_name AS total";
    $total = $wpdb->get_var( $total_query );
    $get_qa = $wpdb->get_results("select * from $your_table_name where your_column_name ='unapproved' LIMIT $offset, $items_per_page");
    print '<div class="wrap">';
    print "<h1>$title</h1>";
    
    echo "<table class='wp-list-table widefat fixed striped table-view-list comments'>";
    echo "<thead>";
    echo "<tr>";
    // echo "<th style='width: 15px;'><input type='checkbox' class='check_all chk1' id='chk_all' /></th>";
    echo "<th>Name</th>";
    echo "<th>Email</th>";
    echo "<th>Address</th>";
    echo "<th>Phone Number</th>";
    echo "<th>Approve/Disapprove</th>";
    echo "</tr>";
    echo "</thead>";
    echo "<tbody>";
    foreach ($get_qa as $row) {
    
    echo "<tr>";
    echo "<td>$row->Name</td>";
    echo "<td>$row->Email</td>";
    echo "<td>$row->Local_address</td>";
    echo "<td>$row->Number</td>";
    echo "<td>";
    echo '<button id="'.$row->Id.'" class="appoved_user_click">Approve';
    echo '</button>';
    echo '<button id="'.$row->Id.'" class="notappoved_user_click">Disapprove/Delete';
    echo '</button>';
    echo "</td>";
    echo "</tr>";
}

	echo "</tbody>";
	echo "<tfoot>";
	echo "<tr>";
	echo "<th>Name</th>";
	echo "<th>Email</th>";
	echo "<th>Address</th>";
	echo "<th>Phone Number</th>";
	echo "<th>Approve/Disapprove</th>";
	echo "</tr>";
	echo "</tfoot>";
	echo "</table>";
	echo '<div class="tablenav-pages">';
	echo paginate_links( array(
	    'base' => add_query_arg( 'cpage', '%#%' ),
	    'format' => '',
	    'prev_text' => __('&laquo;'),
	    'next_text' => __('&raquo;'),
	    'total' => ceil($total / $items_per_page),
	    'current' => $page
	));
	echo '</div>';
	print '</div>';
}
